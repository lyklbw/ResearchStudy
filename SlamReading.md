# Slam阅读笔记

## 第一章：前言

> ***Simultaneous* *Localization and* *Mapping***
>
> 同时定位与地图构建

**书本的核心内容：**

我们会提及SLAM 的历史、理论、算法、现状，并且把完整的SLAM 系统分成几个
模块：视觉里程计、后端优化、建图以及回环检测。我们将陪着读者一点点实现这些模块
中的核心部分，探讨它们在什么情况下有效，什么情况下会出问题，并指导您如何在自己
的机器上运行这些代码。您会接触到一些必要的数学理论和许多编程知识，会用到Eigen、
OpenCV、PCL、g2o、Ceres 等库¬，掌握它们在Linux 操作系统中的使用方法。

代码部分：

https://github.com/gaoxiang12/slambook

## 第二章：初始Slam

那些携带于机器人本体上的传感器，比如激光传感器、相机、轮式编码器、惯性测量单元，它们测到的通常都是一些间接的物理量而不是直接的位置数据。例如，轮式编码器会测到轮子转动的角度、IMU 测量运动的角速度和加速度，相机和激光则读取外部环境的某种观测数据。我们只能通过一些间接的手段，从这些数据推算自己的位置。虽然这听上去是一种迂回战术，但更明显的好处是，它没有对环境提出任何要求，使得这种定位方案**可适用于未知环境**。



**视觉slam使用相机作为主力传感器**

照片：以二维的形式反映了三维的世界，在这个过程丢掉了场景的一个维度：也就是所谓的深度（或距离）。这个距离将是SLAM 中非常关键的信息。由于我们人类见过大量的图像，养成了一种天生的直觉，对大部分场景都有一个直观的距离感（空间感）。

**单目相机**

由于单目相机只是三维空间的二维投影，所以，如果我们真想恢复三维结构，必须移动相机的视角。在单目SLAM 中也是同样的原理。我们必须移动相机之后，才能估计它的运动（Motion），同时估计场景中物体的远近和大小，不妨称之为结构（Structure）

我们知道近处的物体移动快，远处的物体则运动缓慢。于是，当相机移动时，这些物体在图像上的运动，形成了视差。通过视差，我们就能定量地判断哪些物体离得远，哪些物体离的近。

如果把相机的运动和场景大小同时放大两倍，单目所看到的像是一样的。同样的，把这个大小乘以任意倍数，我们都将看到一样的景象。所以单目SLAM 估计的轨迹和地图，将与真实的轨迹、地图，相差一个因子，也就是所谓的尺度，由于单目SLAM 无法仅凭图像确定这个真实尺度，所以又称为尺度不确定性。

**双目相机和深度相机**

**双目相机**

双目相机由两个单目相机组成，但这两个相机之间的距离（称为基线（Baseline））是已知的。但是计算机上的双目相机需要大量的计算才能（不太可靠地）估计每一个像素点的深度。双目相机测量到的深度范围与基线相关。基线距离越大，能够测量到的就越远

双目或多目相机的缺点是配置与标定均较为复杂，其深度量程和精度受双目的基线与分辨率限制，而且视差的计算非常消耗计算资源

**深度相机**

> 又称RGB-D 相机

它最大的特点是可以通过红外结构光或Time-of-Flight（ToF）原理，像激光传感器那样，通过主动向物体发射光并接收返回的光，测出物体离相机的距离。

不过，现在多数RGB-D 相机还存在测量范围窄、噪声大、视野小、易受日光干扰、无法测量透射材质等诸多问题，在SLAM 方面，主要用于室内SLAM，室外则较难应用。



**SLAM框架**

**流程视图**

<img src="C:\Users\15129\AppData\Roaming\Typora\typora-user-images\image-20231112002657992.png" alt="image-20231112002657992" style="zoom:50%;" />

**基本解释**

1.传感器信息读取。在视觉SLAM 中主要为相机图像信息的读取和预处理。如果在机
器人中，还可能有码盘、惯性传感器等信息的读取和同步。

2.视觉里程计(Visual Odometry, VO)。视觉里程计任务是估算相邻图像间相机的运动，
以及局部地图的样子。VO 又称为前端（Front End）。

3.后端优化（Optimization）。后端接受不同时刻视觉里程计测量的相机位姿，以及回
环检测的信息，对它们进行优化，得到全局一致的轨迹和地图。由于接在VO 之后，
又称为后端（Back End）

4.回环检测（Loop Closing）。回环检测判断机器人是否曾经到达过先前的位置。如果
检测到回环，它会把信息提供给后端进行处理。

5.建图（Mapping）。它根据估计的轨迹，建立与任务要求对应的地图



### 视觉里程计

> *Visual Odometry, VO*
>
> **视觉里程计关心相邻图像之间的相机运动**

而视觉SLAM 中，我们只能看到一个个像素，知道它们是某些空间点在相机的成像平面上投影的结果，为了定量地估计相机运动，必须在了解相机与空间点的几何关系。



VO 能够通过相邻帧间的图像估计相机运动，并恢复场景的空间结构。

叫它为“里程计”是因为它和实际的里程计一样，只计算相邻时刻的运动，而和再往前的过去的信息没有关联。

假定我们已有了一个视觉里程计，估计了两张图像间的相机运动。那么，只要把相邻时刻的运动“串”起来，就构成了机器人的运动轨迹，从而解决了定位问题。但，仅通过视觉里程计来估计轨迹，将不可避免地出现**累计漂移（Accumulating Drift）**。这是由于视觉里程计（在最简单的情况下）只估计两个图像间运动造成的。每次估计都带有一定的误差，而由于里程计的工作方式，<u>先前时刻的误差将会传递到下一时刻</u>，导致经过一段时间之后，估计的轨迹将不再准确.



为了解决漂移问题，我们还需要两种技术：**后端优化和回环检测**。

回环检测负责把“机器人回到原始位置”的事情检测出来，而后端优化则根据该信息，校正整个轨迹的形状



### 后端优化

> ***back end***
>
> **后端优化主要指处理SLAM 过程中噪声的问题**

笼统地,后端优化主要指处理SLAM 过程中噪声的问题(传感器测量噪声)

后端优化要考虑的问题，就是如何从这些带有噪声的数据中，估计整个系统的状态，以及这个状态估计的不确定性有多大——这称为最大后验概率估计（Maximum-a-Posteriori，MAP）

<u>视觉 SLAM 中，前端和计算机视觉研究领域更为相关，比如图像的特征提取与匹配等，后端则主要是滤波与非线性优化算法</u>

> SLAM 问题的本质：对运动主体自身和周围环境空间不确定性的估计。

### 回环检测

> ***Loop Closure Detection***
>
> **主要解决位置估计随时间漂移的问题**

假设实际情况下，机器人经过一段时间运动后<u>回到了原点，但是由于漂移，它的位置估计值却没有回到原点</u>。怎么办呢？我们想，如果有某种手段，让机器人**<u>知道</u>**“回到了原点”这件事，或者把“原点”识别出来，我们再把位置估计值“拉”过去，就可以消除漂移了。这就是所谓的回环检测。

回环检测与“定位”和“建图”二者都有密切的关系。事实上，我们认为，地图存在的主要意义，是为了让机器人知晓自己到达过的地方。为了实现回环检测，我们需要让机器人具有识别曾到达过的场景的能力。可以设置标志物等，但是我们更希望机器人能使用携带的传感器——也就是图像本身，来完成这一任务。比如通过图像的相似性，通过辨别进行匹配识别，同样可以减少累计误差。

所以视觉回环检测，**<u>实质上是一种计算图像数据相似性的算法</u>**。由于图像的信息非常丰富，使得正确检测回环的难度也降低了不少。

在检测到回环之后，我们会把“A 与B 是同一个点”这样的信息告诉后端优化算法。然后，后端根据这些新的信息，把轨迹和地图调整到符合回环检测结果的样子。这样，如果我们有充分而且正确的回环检测，就可以消除累积误差，得到全局一致的轨迹和地图

### 建图

> ***Mapping***
>
> **是指构建地图的过程。地图是对环境的描述**

建图根据实际情况的不同灵活变换，但是大体上可以分为可以分为**度量地图**与**拓扑地图**两种

**度量地图**

> ***Metric* *Map***
>
> **度量地图强调精确地表示地图中物体的位置关系**

通常我们用稀疏（Sparse）与稠密（Dense）对***Metric Map**进*行分类

***Sparse***

并不需要表达所有的物体。例如，我们选择一部分具有代表意义的东西，称之为路标（Landmark）,一张稀疏地图就是由路标组成的地图，而不是路标的部分就可以忽略掉.

***Dense***

稠密地图着重于建模所有看到的东西。对于定位来说，稀疏路标地图就足够了,但是对于导航，我们往往需要稠密的地图

稠密地图通常按照某种分辨率，由许多个小块组成。二维度量地图是许多个小格子（Grid），三维则是许多小方块（Voxel）

一般地，一个小块含有占据、空闲、未知三种状态，以表达该格内是否有物体。

但是，这种地图需要存储每一个格点的状态，耗费大量的存储空间，而且多数情况下地图的许多细节部分是无用的。另一方面，大规模度量地图有时会出现一致性问题。很小的一点转向误差，可能会导致两间屋子的墙出现重叠，使得地图失效。




**拓扑地图**

> ***Topological Map***
>
> **相比于度量地图的精确性，拓扑地图则更强调地图元素之间的关系。拓扑地图是一个**
> **图（Graph），由节点和边组成，只考虑节点间的连通性**

它放松了地图对精确位置的需要，去掉地图的细节问题，是一种更为紧凑的表达方式.然而，拓扑地图不擅长表达具有复杂结构的地图。如何对图进行分割形成结点与边，又如何使用拓扑地图进行导航与路径规划，仍是有待研究的问题。



### 数学表述（2.3）

直接看书上对应的板块。



### 编程实战

> 书本的代码运行在Ubuntu14.04的系统下

**cmake的使用**

cmake用来管理工程文件之间的关系

```shell
//mkdir一个bulid的文件，存放自动生成的中间文件
mkdir bulid 
cd build 
cmake ..
make
```

**使用库**

在Linux 中，库文件分成静态库和共享库两种¬。静态库以.a 作为后缀名，共享库以.so
结尾。所有库都是一些函数打包后的集合，差别在于静态库每次被调用都会生成一个副本，
而共享库则只有一个副本，更省空间。

```cmake
//这条命令告诉cmake，我想把这个文件编译成一个叫做“hello”的库
add_library(hello libHelloSLAM.cpp)
//生成共享库
add_library(hello_shared SHARED libHelloSLAM.cpp)
```

库文件是一个压缩包，里头带有编译好的二进制函数。不过，仅有.a 或.so 库文件的
话，我们并不知道它里头的函数到底是什么，调用的形式又是什么样的。为了让别人（或
者自己）使用这个库，我们需要提供一个头文件，说明这些库里都有些什么。因此，对于库
的使用者，只要拿到了头文件和库文件，就可以调用这个库了。

```C++
#ifndef LIBHELLOSLAM_H_
#define LIBHELLOSLAM_H_
void printHello();
#endif
```

然后，在CMakeLists.txt 中添加一个可执行程序的生成命令，链接到刚才我们使用的
库上

```cmake
add_executable(useHello useHello.cpp)
target_link_libraries( useHello hello_shared )
```

通过这两句话，useHello 程序就能顺利使用hello_shared 库中的代码了。

## 第三章：三维刚体运动

> 本节目标
> 1. 理解三维空间的刚体运动描述方式：旋转矩阵、变换矩阵、四元数和欧拉角。
> 2. 掌握***Eigen*** 库的矩阵、几何模块使用方法。

### ***Eigen***

*Eigen*提供了*C++ 中的矩阵运算，并且它的Geometry 模块还提供了四元数等刚
体运动的描述。Eigen 的优化非常完善，但是它的使用方法有一些特殊的地方，我们会在
程序中介绍。

使用Eigen库如何改写CMakeLists.txt?

```
#头文件目录
include_directories("E:\\Loaddown\\eigen-3.4.0\\eigen-3.4.0")
```

相比于其他库，Eigen 特殊之处在于，它是一个纯用头文件搭建起来的库（这非常神奇！）。这意味着你只能找到它的头文件，而没有.so 或.a 那样的二进制文件。我们在使用时，只需引入Eigen 的头文件即可，不需要链接它的库文件（因为它没有库文件）

```C++
/****************************
* 本程序演示了 Eigen 基本类型的使用
****************************/
#include <iostream>
using namespace std;
#include <ctime>
// Eigen 部分
#include <Eigen/Core>
// 稠密矩阵的代数运算（逆，特征值等）
#include <Eigen/Dense>

#define MATRIX_SIZE 50

int main( int argc, char** argv )
{
    // Eigen 中所有向量和矩阵都是Eigen::Matrix，它是一个模板类。它的前三个参数为：数据类型，行，列
    // 声明一个2*3的float矩阵
    Eigen::Matrix<float, 2, 3> matrix_23;

    // 同时，Eigen 通过 typedef 提供了许多内置类型，不过底层仍是Eigen::Matrix
    // 例如 Vector3d 实质上是 Eigen::Matrix<double, 3, 1>，即三维向量
    Eigen::Vector3d v_3d;
    // 这是一样的
    Eigen::Matrix<float,3,1> vd_3d;

    // Matrix3d 实质上是 Eigen::Matrix<double, 3, 3>
    Eigen::Matrix3d matrix_33 = Eigen::Matrix3d::Zero(); //初始化为零
    // 如果不确定矩阵大小，可以使用动态大小的矩阵
    Eigen::Matrix< double, Eigen::Dynamic, Eigen::Dynamic > matrix_dynamic;
    // 更简单的
    Eigen::MatrixXd matrix_x;
    // 这种类型还有很多，我们不一一列举

    // 下面是对Eigen阵的操作
    // 输入数据（初始化）
    matrix_23 << 1, 2, 3, 4, 5, 6;
    // 输出
    cout << matrix_23 << endl;

    // 用()访问矩阵中的元素
    for (int i=0; i<2; i++) {
        for (int j=0; j<3; j++)
            cout<<matrix_23(i,j)<<"\t";
        cout<<endl;
    }

    // 矩阵和向量相乘（实际上仍是矩阵和矩阵）
    v_3d << 3, 2, 1;
    vd_3d << 4,5,6;
    // 但是在Eigen里你不能混合两种不同类型的矩阵，像这样是错的(是因为double和float的问题)
    // Eigen::Matrix<double, 2, 1> result_wrong_type = matrix_23 * v_3d;
    // 应该显式转换
    Eigen::Matrix<double, 2, 1> result = matrix_23.cast<double>() * v_3d;
    cout << result << endl;

    Eigen::Matrix<float, 2, 1> result2 = matrix_23 * vd_3d;
    cout << result2 << endl;

    // 同样你不能搞错矩阵的维度
    // 试着取消下面的注释，看看Eigen会报什么错
    // Eigen::Matrix<double, 2, 3> result_wrong_dimension = matrix_23.cast<double>() * v_3d;

    // 一些矩阵运算
    // 四则运算就不演示了，直接用+-*/即可。
    matrix_33 = Eigen::Matrix3d::Random();      // 随机数矩阵
    cout << matrix_33 << endl << endl;
//各类基本函数操作
    cout << matrix_33.transpose() << endl;      // 转置
    cout << matrix_33.sum() << endl;            // 各元素和
    cout << matrix_33.trace() << endl;          // 迹 trace主对角线元素之和的比喻特征值之和
    cout << 10*matrix_33 << endl;               // 数乘
    cout << matrix_33.inverse() << endl;        // 逆inverse
    cout << matrix_33.determinant() << endl;    // 行列式determinate

    // 特征值
    // 实对称矩阵可以保证对角化成功
    Eigen::SelfAdjointEigenSolver<Eigen::Matrix3d> eigen_solver ( matrix_33.transpose()*matrix_33 );
    cout << "Eigen values = \n" << eigen_solver.eigenvalues() << endl;
    cout << "Eigen vectors = \n" << eigen_solver.eigenvectors() << endl;

    // 解方程
    // 我们求解 matrix_NN * x = v_Nd 这个方程
    // N的大小在前边的宏里定义，它由随机数生成
    // 直接求逆自然是最直接的，但是求逆运算量大

    Eigen::Matrix< double, MATRIX_SIZE, MATRIX_SIZE > matrix_NN;
    matrix_NN = Eigen::MatrixXd::Random( MATRIX_SIZE, MATRIX_SIZE );
    Eigen::Matrix< double, MATRIX_SIZE,  1> v_Nd;
    v_Nd = Eigen::MatrixXd::Random( MATRIX_SIZE,1 );

    clock_t time_stt = clock(); // 计时，以微秒为单位
    // 直接求逆
    Eigen::Matrix<double,MATRIX_SIZE,1> x = matrix_NN.inverse()*v_Nd;
    cout <<"time use in normal inverse is " << 1000* (clock() - time_stt)/(double)CLOCKS_PER_SEC << "ms"<< endl;

    // 通常用矩阵分解来求，例如QR分解，速度会快很多
    time_stt = clock();
    x = matrix_NN.colPivHouseholderQr().solve(v_Nd);//使用奇异值分解（QR 分解）求解线性方程组，v_Nd是等号右侧向量
    cout <<"time use in Qr decomposition is " <<1000*  (clock() - time_stt)/(double)CLOCKS_PER_SEC <<"ms" << endl;

    return 0;
}
```



注：

1.Eigen 提供的矩阵和MATLAB 很相似，几乎所有的数据都当作矩阵来处理。但是，为了实现更好的效率，在Eigen 中你需要指定矩阵的大小和类型。对于在编译时期就知道大小的矩阵，处理起来会比动态变化大小的矩阵更快一些。因此，像旋转矩阵、变换矩阵这样的数据，完全可在编译时期确定它们的大小和数据类型。

2.Eigen 矩阵不支持自动类型提升，这和C++ 的内建数据类型有较大差异。在C++程序中，我们可以把一个float 数据和double 数据相加、相乘，编译器会自动把数据类型转换为最合适的那种。而在Eigen 中，出于性能的考虑，必须显式地对矩阵类型进行转换。

更多关于Eigen的内容

http://eigen.tuxfamily.org/dox-devel/modules.html





### 旋转向量

变换矩阵用十六个量表达了六自由度的变换

旋转矩阵自身带有约束：它必须是个正交矩阵（保持原向量的长度不变），且行列式为1。变换矩阵也是如此。当我们想要估计或优化一个旋转矩阵/变换矩阵时，这些约束会使得求解变得更困难。

任意旋转都可以用一个旋转轴和一个旋转角来刻画。于是，我们可以使用一个
向量，其方向与旋转轴一致，而长度等于旋转角。

这种向量，称为旋转向量（或轴角，***Axis-Angle***）。这种表示法只需一个三维向量即可描述旋转。同样，对于变换矩阵，我们使用一个旋转向量和一个平移向量即可表达一次变换。这时的维数正好是六维。

那么旋转向量和旋转矩阵之间如何转化？

由罗德里格斯公式(***Rodrigues’s Formula***)

https://zhuanlan.zhihu.com/p/451579313

一言以蔽之就是已知原向量，转轴和转动角度，通过向量分解和数学方法推导出来

### 欧拉角

上述旋转对于人类而言并不直观，欧拉角可以直观地展现物体的旋转——它使用了三个分离的转角，把一个旋转分解成三次绕不同轴的旋转。

你或许在航空、航模中听说过“俯仰角”、“偏航角”这些词。欧拉角当中比较常用的一种，便是用“偏航-俯仰-滚转”（yaw-pitch-roll）三个角度来描述一个旋转的。假设一个刚体的前方（朝向我们的方向）为X 轴，右侧为Y 轴，上方为Z 轴

1. 绕物体的Z 轴旋转，得到偏航角yaw；
2. 绕旋转之后的Y 轴旋转，得到俯仰角pitch；
3. 绕旋转之后的X 轴旋转，得到滚转角roll。

此时，我们可以使用  ***[r; p; y]T*** 这样一个三维的向量描述任意旋转。rpy 角的旋转顺序是ZYX。

ps:值得一提的是，大部分领域在使用欧拉角时有各自的坐标方向和顺序上的习惯，不一定和我们这里说的相同。

欧拉角的一个重大缺点是会碰到著名的万向锁问题（Gimbal Lock¬）：在俯仰角为90◦ 时，第一次旋转与第三次旋转将使用同一个轴，使得系统丢失了一个自由度（由三次旋转变成了两次旋转）这被称为奇异性问题，在其他形式的欧拉角中也同样存在

> **由于这种原理，欧拉角不适于插值和迭代，往往只用于人机交互中。**



### 四元数

> 旋转矩阵用九个量描述三自由度的旋转，具有冗余性；欧拉角和旋转向量是紧凑的，但
> 具有奇异性。事实上，我们找不到不带奇异性的三维向量描述方式。这有点类似于，当
> 我们想用两个坐标表示地球表面时（如经度和纬度），必定存在奇异性（纬度为90◦ 时经
> 度无意义）。三维旋转是一个三维流形，想要无奇异性地表达它，用三个量是不够的。

复数的乘法能表示复平面上的旋转,乘上复数 ***i*** 相当于逆时针把一个复向量旋转90 度

> 第一遍看书没怎么看懂

在表达三维空间旋转时，也有一种类似于复数的代数：四元数（Quaternion）。四元数是Hamilton 找到的一种扩展的复数. 它既是紧凑的，也没有奇异性。如果说缺点的话，四元数不够直观，其运算稍为复杂一些。

蛮抽象的——目前只是会计算了，没有深层次的理解呢



### 相似、仿射、射影变换

只是几个概念而已

<img src="C:\Users\15129\AppData\Roaming\Typora\typora-user-images\image-20231115232602791.png" alt="image-20231115232602791" style="zoom: 50%;" />



### 代码实操

```C++
#include <iostream>
#include <cmath>
using namespace std;

#include <Eigen/Core>
// Eigen 几何模块
#include <Eigen/Geometry>

/****************************
* 本程序演示了 Eigen 几何模块的使用方法
****************************/

int main ( int argc, char** argv )
{
    // Eigen/Geometry 模块提供了各种旋转和平移的表示
    // 3D 旋转矩阵直接使用 Matrix3d 或 Matrix3f
    Eigen::Matrix3d rotation_matrix = Eigen::Matrix3d::Identity();
    // 旋转向量使用 AngleAxis, 它底层不直接是Matrix，但运算可以当作矩阵（因为重载了运算符）
    Eigen::AngleAxisd rotation_vector ( M_PI/4, Eigen::Vector3d ( 0,0,1 ) );     //沿 Z 轴旋转 45 度
    cout .precision(3);
    cout<<"rotation matrix =\n"<<rotation_vector.matrix() <<endl;                //用matrix()转换成矩阵
    // 也可以直接赋值
    rotation_matrix = rotation_vector.toRotationMatrix();
    // 用 AngleAxis 可以进行坐标变换
    Eigen::Vector3d v ( 1,0,0 );
    Eigen::Vector3d v_rotated = rotation_vector * v;
    cout<<"(1,0,0) after rotation = "<<v_rotated.transpose()<<endl;
    // 或者用旋转矩阵
    v_rotated = rotation_matrix * v;
    cout<<"(1,0,0) after rotation = "<<v_rotated.transpose()<<endl;

    // 欧拉角: 可以将旋转矩阵直接转换成欧拉角
    Eigen::Vector3d euler_angles = rotation_matrix.eulerAngles ( 2,1,0 ); // ZYX顺序，即roll pitch yaw顺序
    cout<<"yaw pitch roll = "<<euler_angles.transpose()<<endl;

    // 欧氏变换矩阵使用 Eigen::Isometry
    Eigen::Isometry3d T=Eigen::Isometry3d::Identity();                // 虽然称为3d，实质上是4＊4的矩阵
    T.rotate ( rotation_vector );                                     // 按照rotation_vector进行旋转
    T.pretranslate ( Eigen::Vector3d ( 1,3,4 ) );                     // 把平移向量设成(1,3,4)
    cout << "Transform matrix = \n" << T.matrix() <<endl;

    // 用变换矩阵进行坐标变换
    Eigen::Vector3d v_transformed = T*v;                              // 相当于R*v+t
    cout<<"v tranformed = "<<v_transformed.transpose()<<endl;

    // 对于仿射和射影变换，使用 Eigen::Affine3d 和 Eigen::Projective3d 即可，略

    // 四元数
    // 可以直接把AngleAxis赋值给四元数，反之亦然
    Eigen::Quaterniond q = Eigen::Quaterniond ( rotation_vector );
    cout<<"quaternion = \n"<<q.coeffs() <<endl;   // 请注意coeffs的顺序是(x,y,z,w),w为实部，前三者为虚部
    // 也可以把旋转矩阵赋给它
    q = Eigen::Quaterniond ( rotation_matrix );
    cout<<"quaternion = \n"<<q.coeffs() <<endl;
    // 使用四元数旋转一个向量，使用重载的乘法即可
    v_rotated = q*v; // 注意数学上是qvq^{-1}
    cout<<"(1,0,0) after rotation = "<<v_rotated.transpose()<<endl;

    return 0;
}
```

```
• 旋转矩阵（3  3）：Eigen::Matrix3d。
• 旋转向量（3  1）：Eigen::AngleAxisd。
• 欧拉角（3  1）：Eigen::Vector3d。
• 四元数（4  1）：Eigen::Quaterniond。
• 欧氏变换矩阵（4  4）：Eigen::Isometry3d。
• 仿射变换（4  4）：Eigen::Affine3d。
• 射影变换（4  4）：Eigen::Projective3d。
```

