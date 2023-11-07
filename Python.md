

# Python

## 第一部分 基础知识

在Python中，用引号括起的都是字符串，其中的引号可以是单引号，也可以是双引号

大小写

```py
name="Adele lovesong"
print(name.title()) //首字母大写
print(name.upper()) //全体大写
print(name.lower()) //全体小写
```

合并 拼接

```py
first="adele"
second="lovesong"
add= first +" "+ second;
```

删除空白

```py
name=" python is "
name1=name.rstrip()
name2=name.lstrip();
name3=name.strip();
```

str()

```py
age = 23
message = "Happy " + str(age) + "rd Birthday!"
print(message)
```

python之禅

```py
import this
```



### 列表

列表 由一系列按特定顺序排列的元素组成。你可以创建包含字母表中所有字母、数字0~9或所有家庭成员姓名的列表；也可以将任何东西加入列表中，其中的元素之间可以没有任何关系

列表是有序集合，因此要访问列表的任何元素，只需将该元素的位置或索引告诉Python即可。要访问列表元素，可指出列表的名称，再指出元素的索引，并将其放在方括号内

list[0] 指的是第一个 list[-1]是指最后一个

末尾添加

```py
list.append('ducati')
```

插入元素

```py
moto.insert(0,'sakura')
```

##### 删除

1.`del`如果知道要删除的元素在列表中的位置，可使用del 语句。

```py
del moto[0]
```

2.`pop` 你要将元素从列表中删除，并接着使用它的值。

````
popped_motorcycle = motorcycles.pop() //最后一个元素
````

实际上，你可以使用pop() 来删除列表中任何位置的元素，只需在括号中指定要删除的元素的索引即可。

3.`remove` 

你不知道要从列表中删除的值所处的位置。如果你只知道要删除的元素的值，可使用方法remove()

````py
motorcycles.remove('ducati')
````

组织

`sort` 对小写字母实现排序 为永久性修改

```py
cars = ['bmw', 'audi', 'toyota', 'subaru']//标准列表形式
cars.sort()
```

```py
car.sort（reverse=True）//倒序
```

`sorted` 临时排序

```py
print(sorted(cars))
```

`reverse`

```python
cars.reverse()
```

`len`

```py
len(cars) #cars为列表
```

##### 循环（for)

python中要注意缩进，缩进表示了代码的从属关系

基本操作

```py
mac=['a','b','c']
for x in mac: #注意注意这个：
	print(x)	 
```

更多

```py
magicians = ['alice', 'david', 'carolina']
for magician in magicians:
	print(magician.title() + ", that was a great trick!")
	print("I can't wait to see your next trick, " + magician.title() + ".\n")
print(123)
```

`range()`函数

```py
#使用range创造列表
num=list(range(1,6))
print(num)
```

三个参数

```py
#函数range() 从2开始数，然后不断地加2，直到达到或超过终值（11）
even_num=list(range(2,11,2))
print(even_num);
```

`range`函数的功能可以有很多扩展

```py
squares = []
for value in range(1,11):
 square = value**2
 squares.append(square)
 print(squares)
# ps:其实可以直接append
```

 使用子集

要提取列表的第2~4个元素，可将起始索引指定为1 ，并将终止索引指定为4

```py
players = ['charles', 'martina', 'michael', 'florence', 'eli']
print(players[1:4]) #此处依旧是以0打头
```

如果你没有指定第一个索引，Python将自动从列表开头开始：

```py
#本质是省略什么就默认最值
players = ['charles', 'martina', 'michael', 'florence', 'eli']
print(players[:4])
```

之前说过：负数索引返回离列表末尾相应距离的元素，因此你可以输出列表末尾的任何切片。

```py
#输出名单最后三个人
players = ['charles', 'martina', 'michael', 'florence', 'eli']
print(players[-3:])
```

列表复制

```py
#使用切片
my_foods = ['pizza', 'falafel', 'carrot cake']
friend_foods = my_foods[:]

#倘若我们只是简单地将my_foods 赋给friend_foods ，就不能得到两个列表。例如，下例演示了在不使用切片的情况下复制列表的情况：
friend_foods = my_foods
#这种语法实际上是让Python将新变量friend_foods 关联到包含
在my_foods 中的列表，因此这两个变量都指向同一个列表。
```





### 元组

列表是可以修改的，这对处理网站的用户列表或游戏中的角色列表至关重要。然而，有时候你需要创建一系列不可修改的元素，元组可以满足这种需求。Python将不能修改的值称为不可变的 ，而不可变的列表被称为元组

元组看起来犹如列表，但使用圆括号而不是方括号来标识。定义元组后，就可以使用索引来访问其元素，就像访问列表元素一样。

```py
dimensions = (200, 50)
for dimension in dimensions:
print(dimension)
```



虽然不能修改元组的元素，但可以给存储元组的变量赋值。因此，如果要修改前述矩形的尺寸，可重新定义整个元组：

```py
dimensions = (200, 50)
print("Original dimensions:")
for dimension in dimensions:
	print(dimension)
dimensions = (400, 100)
print("\nModified dimensions:")
for dimension in dimensions:
	print(dimension)	
```



### if 语句

各种比较本质上和c语言差不太多 **要加`：`**

但是或和与被 or 和 and 代替了



检查特定值的是否包含在列表中

```py
>>> requested_toppings = ['mushrooms', 'onions', 'pineapple']
>>> 'mushrooms' in requested_toppings
True
#检查在不在
if user in banned_users
#同理 一样的有检查不在
if user not in banned_users:
```

同样有布尔表达式

True

False

```py
#if elif else 用法
❶ if age < 4:
print("Your admission cost is $0.")
❷ elif age < 18:
print("Your admission cost is $5.")
❸ else:
print("Your admission cost is $10.")
```

使用多个列表的例子

```py
available_toppings = ['mushrooms', 'olives', 'green peppers',
'pepperoni', 'pineapple', 'extra cheese']
requested_toppings = ['mushrooms', 'french fries', 'extra cheese']
for requested_topping in requested_toppings:
	if requested_topping in available_toppings:
		print("Adding " + requested_topping + ".")
	else:
		print("Sorry, we don't have " + requested_topping + ".")
print("\nFinished making your pizza!")
```



### 字典

更大限度的存储信息

在Python中，字典 是一系列键 —值对 。每个键 都与一个值相关联，你可以使用键来访问与之相关联的值。与键相关联的值可以是数字、字符串、列表乃至字典。事实上，可将
任何Python对象用作字典中的值

Python不关心键—值对的添加顺序，而只关心键和值之间的关联关系

```py
#创建空字典  使用花括号
alien={}
#添加元素
alien_0 = {'color': 'green', 'points': 5}
print(alien_0)
alien_0['x_position'] = 0//不存在的键直接生成并加入字典
alien_0['y_position'] = 25
```

使用`del`删除键值对

key value 一个键 一个值

遍历字典

```py
user_0 = {
'username': 'efermi',
'first': 'enrico',
'last': 'fermi',
}
#items函数返回键值对 需要两个元素接收
for key,value in user_0.items():
	print("\n"+key)
	print(value)
```

注意，即便遍历字典时，键—值对的返回顺序也与存储顺序不同

`.keys()`返回所有键



按顺序遍历字典的所有值：

```py
#sorted一下
favorite_languages = {
'jen': 'python',
'sarah': 'c',
'edward': 'ruby',
'phil': 'python',
}
for name in sorted(favorite_languages.keys()):
	print(name.title() + ", thank you for taking the poll.")
```

剔除重复项

```py
#通过对包含重复元素的列表调用set() ，可让Python找出列表中独一无二的元素，并使用这些元素来创建一个集合。
for language in set(favorite_languages.values()):
	print(language.title())
```



简单嵌套

```py
#创建含三个外星人(字典)的列表
alien_0 = {'color': 'green', 'points': 5}
alien_1 = {'color': 'yellow', 'points': 10}
alien_2 = {'color': 'red', 'points': 15}
aliens = [alien_0, alien_1, alien_2]
for alien in aliens:
	print(alien)
```

三十个

```py
aliens = []
for alien_num in range(30):#range(x),由0开始起x个
	new_alien={'color': 'green', 'points': new_alien, 'speed': 'slow'}
	aliens.append(new_alien)
#显示前五个
for alien in aliens[0:29]:
	print(alien)
print("...")
```



在字典中存储列表

```py
#字典是{} 列表是[]
pizza = {
'crust': 'thick',
'toppings': ['mushrooms', 'extra cheese'],
}
```

字典中存储字典

```py
users = {
'aeinstein': {
'first': 'albert',
'last': 'einstein',
'location': 'princeton',
},
'mcurie': {
'first': 'marie',
'last': 'curie',
'location': 'paris',
},
}
#访问
for username, user_info in users.items():
	print("\nUsername: " + username)
	full_name = user_info['first'] + " " + user_info['last']
	location = user_info['location']
	print("\tFull name: " + full_name.title())
	print("\tLocation: " + location.title())
```



### 输入

`input()`

函数`input() `让程序暂停运行，等待用户输入一些文本。获取用户输入后，Python将其存储在一个变量中，以方便你使用

`int()`获得数值输入

```py
>>> age = input("How old are you? ")
How old are you? 21
>>> age = int(age)
>>> age >= 18
True
```



### while

关注其`break`与`continue`的使用

```py
current_number = 1
while current_number <= 5: #不要忘记冒号呀呀
print(current_number)
current_number += 1
```

让用户选择退出时机

```py
while message != 'quit':
	message=input(comment)
	print(message)
```



标志（思想）

在要求很多条件都满足才继续运行的程序中，可定义一个变量，用于判断整个程序是否处于活动状态。这个变量被称为标志 ，充当了程序的交通信号灯。



使用while来处理列表和字典

下面的代码虽然非常无用，但是可以学习其细节

````py
# 首先，创建一个待验证用户列表
# 和一个用于存储已验证用户的空列表
unconfirmed_users = ['alice', 'brian', 'candace']
confirmed_users = []
# 验证每个用户，直到没有未验证用户为止
# 将每个经过验证的列表都移到已验证用户列表中
while unconfirmed_users:
	current_user = unconfirmed_users.pop()#注意pop会取出元素哟
	print("Verifying user: " + current_user.title())
	confirmed_users.append(current_user)
# 显示所有已验证的用户
print("\nThe following users have been confirmed:")
for confirmed_user in confirmed_users:
	print(confirmed_user.title())
````

用while删除列表元素

```py
pets = ['dog', 'cat', 'dog', 'goldfish', 'cat', 'rabbit', 'cat']
print(pets)
while 'cat' in pets:
	pets.remove('cat')
print(pets)
```



### 函数

定义函数

```py
def greet_user():
	"""显示简单的问候"""
    print("conijiwa")
greet_user()
```

传参

```py
def function(pet_type,pet_name):
	print("i have a "+pet_type+" named "+pet_name)
#讲究顺序
function('dog','zzf')
```

但是如果是使用关键字实参，直接在实参中将名称和值关联起来了。关键字实参让
你无需考虑函数调用中的实参顺序

```py
def describe_pet(animal_type, pet_name):
"""显示宠物的信息"""
	print("\nI have a " + animal_type + ".")
	print("My " + animal_type + "'s name is " + pet_name.title() + ".")
describe_pet(animal_type='hamster', pet_name='harry')
```

还可以给默认值

在调用函数中给形参提供了实参时，Python将使用指定的实参值；否则，将使用形参的默认

```py
def describe_pet(pet_name, animal_type='dog'):
"""显示宠物的信息"""
	print("\nI have a " + animal_type + ".")
	print("My " + animal_type + "'s name is " + pet_name.title() + ".")
describe_pet(pet_name='willie')	
```



##### 返回值

return啥都很自由（字典列表元组）

结合while使用函数

**<u>注意注意</u>**

```py
import time
def print_machine(unprinted_designs,completed_models):
	"""检验"""

	while unprinted_designs:
		current_designs=unprinted_designs.pop();
		print("printing"+current_designs+ " waiting waiting...\n")
		time.sleep(1)
		print(current_designs+"has been printed")
		completed_models.append(current_designs)

a=['LBJ','KD',"Jordan"]
b=[]
print_machine(a,b)
print('a:')
print(a)
print('\nb:') 
print(b)	
#结果
a:
[]

b:
['Jordan', 'KD', 'LBJ']

```

python的参数变化和c语言有所不同，形式参数（c语言看来的）会被函数所改变。。正如上述例子所展示的

将列表传递给函数后，函数就可对其进行修改。在函数中对这个列表所做的任何修改都是永久性的，这让你能够高效地处理大量的数据。

##### 禁止修改

```py
#使用副本
print_models(unprinted_designs[:], completed_models)
```

##### 传递任意数量的参数

你预先不知道函数需要接受多少个实参，好在Python允许函数从调用语句中收集任意数量的实参。

```py
def  function(*toppings):
	print(toppings)
```

所以*代表创建一个空列表



而**则表示创建一个空字典

```py
def function(pizza_name,**gradient):
	print(pizza_name)
	print(gradient)

function('Italy',cheese="extra",pepper="none")
```



##### 将函数存入模块中

```py
#pizza.py
def make_pizza(size, *toppings):
	"""概述要制作的比萨"""
	print("\nMaking a " + str(size) +
	"-inch pizza with the following toppings:")
	for topping in toppings:
		print("- " + topping)	
```

使用模块

```py
import pizza

pizza.make_pizza(16,"pepper","cheese")
pizza.make_pizza(12,"mushroom")

```

<u>才发现for自带`\n`</u>



导入特定的函数

```python
from pizza import function
#可以重新定义名字（可能出现重名的情况）
from pizza import function as lyk
```



### 类

##### 概述

```py
#学习类 （区分类与实例）
class Dog():

	def __init__(self,name,age):
#创造新实例时，Python会自动调用)_init_()，其中self必不可少
#它是指向实例本身的引用
		self.name=name
		self.age=age
	def sit(self):
		print(self.name.title()+" is now sitting")
	def roll_over(self):
		print(self.name.title()+" rolled over!")
#调用
my_dog= Dog('james',6)

my_dog.sit()

my_dog.roll_over()

print(my_dog.name)
```

类中的函数称为方法 ；你前面学到的有关函数的一切都适用于方法，就目前而言，唯一重要的差别是调用方法的方式。

__init__() 是一个特殊的方法，每当你根据Dog 类创建新实例时，Python都会自动运行它。在这个方法的名称中，<u>开头和末尾各有两个下划线</u>，这是一种约定，旨在避免Python默认方法与普通方法发生名称冲突。

其中形参self 必不可少，还必须位于其他形参的前面

每个与类相关联的方法调用都自动传递实参self ，它是一个指向实例本身
的引用，让实例能够访问类中的属性和方法

```py
#访问属性（基于上述的例子）
my_dog.name
```



##### 使用

```py
class Car():
	def __init__(self,name,age,nation):
		self.name=name
		self.age=age
		self.nation=nation
        self.odometer_reading=0;#指定默认值
	def print_function(self):
		long_name=(self.name + " driven for "+self.age + " years"+" from"+ self.nation)
		print(long_name)
#python不能整型数连接字符串要用 str()转换
my_car=Car("Benz C320","3","German")
my_car.print_function()
```

##### 修改属性的值

1.直接修改

```py
my_car.odometer=100;
```

2.使用方法更新

```py
class Car()
	--snip--
	#传入参数进入函数 实现修改
	def updata_odometer(self,mileage):
		self.odometer=mileage
```

扩展功能

```py
def update_odometer(self, mileage):
"""将里程表读数设置为指定的值  禁止将里程表读数往回调"""
if mileage >= self.odometer_reading:
	self.odometer_reading = mileage
else:
	print("You can't roll back an odometer!")
```

##### 继承

编写类时，并非总是要从空白开始。如果你要编写的类是另一个现成类的特殊版本，可使用继承

一个类继承 另一个类时，它将自动获得另一个类的所有属性和方法；原有的 类称为父类 ，而新类称为子类 。子类继承了其父类的所有属性和方法，同时还可以定义自己的属性和方法。

````py
class electricCar(Car):
	"""电动汽车的特别之处"""
	def __init__(self,name,age,nation):
		super().__init__(name,age,nation)
mytesla=electricCar("tesla","3","the US")
mytesla.print_function()
````

创建子类时，父类必须包含在当前文件中，且位于子类前面

定义子类时，必须在括号内指定父类的名称。

方法__init__() 接受创建Car 实例所需的信息

`super()` 是一个特殊函数，帮助Python将父类和子类关联起来。这行代码让`Python`调用`ElectricCar` 的父类的方法`__init__() `，让`ElectricCar` 实例包含父类的所有属性。父类也称为超类 （`superclass`），名称`super`因此而得名

父类包括普遍特点 子类存储自身特点 可以节省代码



重写父类

对于`car`而言 其有一个名为`fill_gas_tank()`的方法

```py
def ElectricCar(Car):
--snip--
	def fill_gas_tank():
		"""电动汽车没有油箱"""
		print("This car doesn't need a gas tank!")
```

如果有人对电动汽车调用方法fill_gas_tank() ，Python将忽略Car 类中的方法fill_gas_tank() ，转而运行上述代码。使用继承时，可让子类保留从父类那里继承而来的精华，并剔除不需要的糟粕



##### 将实例用作属性

当不断给ElectricCar 类添加细节时，我们可能会发现其中包含很多专门针对汽车电瓶的属性和方法。在这种情况下，我们可将这些属性(变量)和方法（函数）提取出来，放到另一个名为Battery 的类中，并将一个Battery 实例用作ElectricCar 类的一个属性

##### 模拟实物（思想）

解决实际问题时，，要从更高逻辑来解决问题（电池的多种表达方式），考虑的不是语言，而是如何将事物通过代码具象化。不断学习是自己的代码高效化



##### 导入类

随着你不断地给类添加功能，文件可能变得很长，即便你妥善地使用了继承亦如此。为遵循`Python`的总体理念，应让文件尽可能整洁。为在这方面提供帮助，`Python`允许你将类存储在模块中，然后在主程序中导入所需的模块

将`Car` 类存储在一个名为`car.py`的模块中，该模块将覆盖前面使用的文件`car.py`。从现在开始，使用该模块的程序都必须使用更具体的文件名，如`my_car.py`

```py
#从模块中导入一个类
from car import Car
#从模块中导入多个类
from car import Car,ElectricCar
#导入整个模块
import car
#在一个模块中导入另外一个模块
from car import Car

class Battery():
	--snip--
class ElectricCar(Car):
    --snip--
```

##### 类编码风格

类名应采用驼峰命名法 ，即将类名中的每个单词的首字母都大写，而不使用下划线。实例名和模块名都采用小写格式，并在单词之间加上下划线

对于每个类，都应紧跟在类定义后面包含一个文档字符串。这种文档字符串简要地描述类的功能，并遵循编写函数的文档字符串时采用的格式约定。每个模块也都应包含一个文档字符串，对其中的类可用于做什么进行描述。

需要同时导入标准库中的模块和你编写的模块时，先编写导入标准库模块的import 语句，再添加一个空行，然后编写导入你自己编写的模块的import 语句。在包含多条import 语句的程序中，这种做法让人更容易明白程序使用的各个模块都来自何方。

### Python库

实例：`OrderedDict`

````py
from collections import OrderedDict
favorite_language = OrderedDict()

favorite_languages['jen'] = 'python'
favorite_languages['sarah'] = 'c'
favorite_languages['edward'] = 'ruby'
favorite_languages['phil'] = 'python'

for name,language in favorite_language.items():
	pirnt(name.title()+"'s favprite language is "+language.title()+".")
````

### 文件！

本章中，你将学习处理文件，让程序能够快速地分析大量的数据；你将学习错误处理，避免程序在面对意外情形时崩溃；你将学习异常 ，它们是Python创建的特殊对象，用于管理程序运行时出现的错误；你还将学习模块`json ` ，它让你能够保存用户数据，以免在程序停止运行后丢失

##### 读取文件

```py
#写一个open就可以了，python会自己判断close的时机
#文件读取
with open('pi_digits.txt') as file_object:
	contents = file_object.read()
	print(contents)
#逐行读取
filename='pi_digits.txt'
with open(filename) as file_object:
    for line in file_object:
        print(line)
print("创建一个包含文件各行内容的列表")
with open(filename) as file_object:
	lines=file_object.readlines()#从文件中读取一行,在with代码块之外 依旧就可以使用
for line in lines:
	print(line.rstrip())
```

在这个程序中，第1行代码做了大量的工作。我们先来看看函数open() 。要以任何方式使用文件——哪怕仅仅是打印其内容，都得先打开文件，这样才能访问它。函数open()接受一个参数：要打开的文件的名称。Python在当前执行的文件所在的目录中查找指定的文件。在这个示例中，当前运行的是file_reader.py，因此Python在file_reader.py所在的目录中查找pi_digits.txt。函数open() 返回一个表示文件的对象。在这里，open('pi_digits.txt') 返回一个表示文件pi_digits.txt 的对象；Python将这个对象存储在我们将在后面使用的变量中。

关键字with 在不再需要访问文件后将其关闭。在这个程序中，注意到我们调用了open() ，但没有调用close() ；你也可以调用open() 和close() 来打开和关闭文件，但这样做时，如果程序存在bug，导致close() 语句未执行，文件将不会关闭。这看似微不足道，但未妥善地关闭文件可能会导致数据丢失或受损。如果在程序中过早地调用close() ，你会发现需要使用文件时它已关闭 （无法访问），这会导致更多的错误。并非在任何情况下都能轻松确定关闭文件的恰当时机，但通过使用前面所示的结构，可让Python去确定：你只管打开文件，并在需要时使用它，Python自会在合适的时候自动将其关闭。

有了表示pi_digits.txt的文件对象后，我们使用方法read() （前述程序的第2行）读取这个文件的全部内容，并将其作为一个长长的字符串存储在变量contents 中。这样，通过打印contents 的值，就可将这个文本文件的全部内容显示出来

##### 写入文件

```py
filename="programming.txt"
with open (filename ,"w") as file_object:
	file_object.write("I love programming")
```

在这个示例中，调用open() 时提供了两个实参。第一个实参也是要打开的文件的名称；第二个实参（'w' ）告诉Python，我们要以写入模式 打开这个文件。打开文件
时，可指定读取模式 （'r' ）、写入模式 （'w' ）、附加模式 （'a' ）或让你能够读取和写入文件的模式（'r+' ）。如果你省略了模式实参，Python将以默认的只读模式打
开文件

### 异常

##### 除法异常

当你认为可能发生了错误时，可编写一个try-except 代码块来处理可能引发的异常。你让Python尝试运行一些代码，并告诉它如果这些代码引发了指定的异常，该怎么办。处理`ZeroDivisionError `异常的`try-except `代码块类似于下面这样：

````py
try:
	print(5/0)
except ZeroDivisionError:
	print("you can not divide by 0")
````

我们将导致错误的代码行print(5/0) 放在了一个try 代码块中。如果try 代码块中的代码运行起来没有问题，Python将跳过except 代码块；如果try 代码块中的代码导致了错误，Python将查找这样的except 代码块，并运行其中的代码，即其中指定的错误与引发的错误相同。

在这个示例中，try 代码块中的代码引发了`ZeroDivisionError `异常，因此Python指出了该如何解决问题的`except` 代码块，并运行其中的代码。这样，用户看到的是一条友
好的错误消息，而不是`traceback`

```py
#简易除法器
print("Give me two numbers, and I'll divide them.")
print("Enter 'q' to quit.")
while True:
	first=input("\nFirst number: ")
	if first=='q':
		break
	second=input("Second number: ")
	if second=='q':
		break
	try:
		answer=int(first)/int(second)	
	except ZeroDivisionError:
		print("you can not divide by 0")
	else:
		print(answer)
```

##### 处理文件读取异常`FileNotFoundError`

```py
try:
    with open(filename) as f_obj:
        contents=f_obj.read()
except FileNotFoundError:
    message="Sorry,the file is not exist."
    print(message)
```

注意：现在小罗把操作系统换回Windows了，在写代码过程中习惯在慢慢改变和适应 同样笔记中也会包含一些关于vscode的知识

##### 分析文本

单个文件

你可以分析包含整本书的文本文件。很多经典文学作品都是以简单文本文件的方式提供的，因为它们不受版权限

```py
#从网上下载txt文件
filename="Alice.txt"
try:
	with open(filename) as f_obj:
		content=f_obj.read()
except FileNotFoundError:
	print(filename+" is not exist in this directory")
else:
	num=len(content.split())
	print("this txt file contains about "+num+" words")
```

使用多个文件

```py
def word_Count(filename):
    try:
        with open(filename) as file:
            contents = file.read()
    except FileNotFoundError:
        print("Sorry, the file " + filename + " does not exist.")
    else:
        words = contents.split()
        num_words = len(words)
        print("The file " + filename + " has about " + str(num_words) + " words.")
        return int(num_words)

all=0
file_list=["Alice.txt","pi_digits.txt","programming.txt"]
for file in file_list:
    all+=word_Count(file)
print(all)
```

在程序失败时一声不吭

try 和 except 语句 可以处理异常

except语句后接入 pass 可以在terminal无输出



### 存储信息

很多程序都要求用户输入某种信息，如让用户存储游戏首选项或提供要可视化的数据。不管专注的是什么，程序都把用户提供的信息存储在列表和字典等数据结构中。用户关闭程序时，你几乎总是要保存他们提供的信息；一种简单的方式是使用模块json 来存储数据。

模块json 让你能够将简单的Python数据结构转储到文件中，并在程序再次运行时加载该文件中的数据。你还可以使用json 在Python程序之间分享数据。更重要的是，JSON数据格式并非Python专用的，这让你能够将以JSON格式存储的数据与使用其他编程语言的人分享。这是一种轻便格式，很有用，也易于学习。

```py
import json

number=[1,2,3,4,5,6,7,8,9,10]

filename='number.json'

with open(filename,'w') as f_obj:
    json.dump(number,f_obj)#数json.dump() 接受两个实参：要存储的数据以及可用于存储数据的文件对象
```

这个程序没有输出，但我们可以打开文件`numbers.json`，看看其内容。数据的存储格式与Python中一样

下面再编写一个程序，使用`json.load()` 将这个列表读取到内存中：

```py
import json
filename="number.json"
with open(filename) as f_obj:
	 number=json.load(f_obj)
print(number)
```

我们将尝试从文件`username.json`中获取用户名，因此我们首先编写一个尝试恢复用户名的try 代码块。如果这个文件不存在，我们就在except 代码块中提示用户输入用户名，并将其存储在`username.json`中，以便程序再次运行时能够获取它

```py
import json
filename="username.json"

try:
    with open(filename) as f_obj:
        username=json.load(f_obj)
except FileNotFoundError:
    username=input("What is your name?\n")
    with open(filename,'a') as f_obj:    
        json.dump(username,f_obj)
        print("We'll remember you when you come back, "+username+"!")
else:
    print("Welcome back, "+username+"!")
#先try 如果没有问题就是进入else 如果有问题就是进入except
```



### 重构

你经常会遇到这样的情况：代码能够正确地运行，但可做进一步的改进——将代码划分为一系列完成具体工作的函数。这样的过程被称为

重构。重构让代码更清晰、更易于理解、更容易扩展。

上方代码重构之后：

```py
import json
def get_storage():
    file_name="username.json"
    try:
        with open(file_name) as file:
            content=json.load(file)
    except FileNotFoundError:
        return None
    else:
        return content
    
def get_new_storage():
    username=input("What is your name?")
    file_name="username.json"
    with open(file_name,"w") as file:
        json.dump(username,file)
    return username

def greet_user():
    username=get_storage()
    if username:
        print("Welcome back "+username)
    else:
        username=get_new_storage()
        print("We will remember you when you come back "+username)

greet_user()
```



### 测试代码

Python标准库中的模块`unittest `提供了代码测试工具。

单元测试 用于核实函数的某个方面没有问题；

测试用例是一组单元测试，这些单元测试一起核实函数在各种情形下的行为都符合要求。良好的测试用例考虑到了函数可能收到的各种输入，包含针对所有这些情形的测试

为了学习测试代码 我们要先准备一些代码块

接着创建测试用例的语法需要一段时间才能习惯，但测试用例创建后，再添加针对函数的单元测试就很简单了。要为函数编写测试用例，可先导入模块`unittest` 以及要测试的函数，再创建一个继承`unittest.TestCase` 的类，并编写一系列方法对函数行为的不同方面进行测试

##### 测试函数

待测试代码如下：

`name_function.py`

```py
def get_formatted_name(first_name, last_namemid):
    """Generate a neatly formatted full name."""
    full_name = first_name + ' ' + last_namemid
    return full_name.title()
```

测试代码如下：

`test_name_function.py`

```py
import unittest
from name_function import get_formatted_name

class NameTestCacse(unittest.TestCase):
    """测试name_functiom"""
    def test_first_last_name(self):
        """能够正确地处理像Janis Joplin这样的姓名吗？"""
        formatted_name=get_formatted_name("elon","musk")
        self.assertEqual(formatted_name,"Elon Musk")

unittest.main()
```

首先，我们导入了模块`unittest` 和要测试的函数get_formatted_name() 

接着，我们创建了一个名为`NamesTestCase` 的类，用于包含一系列针对get_formatted_name() 的单元测试，这个类必须继承`unittest.TestCase` 类，这样Python才知道如何运行你编写的测试。

`NamesTestCase` 只包含一个方法，用于测试`get_formatted_name() `的一个方面。我们将这个方法命名为`test_first_last_name() `,

`self.assertEqual`是`unittest `类最有用的功能之一：断言功能，用以核实得到的结果是否与期望的结果一致

##### 用于测试的类的函数

| 方法                              | 用途                     |
| --------------------------------- | ------------------------ |
| `assertEqual(a, b) `              | 核实a == b               |
| `assertNotEqual(a, b) `           | 核实a != b               |
| `assertTrue(x) `(True可改为False) | 核实x 为True             |
| `assertIn(item , list ) `         | 核实 *item* 在 *list* 中 |
| `assertNotIn(item , list ) `      | 核实item不在list         |

##### 测试类

待测试代码：

`survey.py`

```py
class AnonymousSurvey():
    """收集匿名调查的问卷"""
    def __init__(self, question):
        """存储一个问题，并为存储答案做准备"""
        self.question = question
        self.responses = []
    
    def show_question(self):
        """显示调查问卷"""
        print(self.question)
    
    def store_response(self, new_response):
        """存储单份调查问卷"""
        if new_response:
            self.responses.append(new_response)
    def show_results(self):
        """显示收集到的所有答卷"""
        print("Survey results:")
        for response in self.responses:
            print("- "+response)
```

测试代码：

`test_survey.py`

```py
import unittest
from survey import AnonymousSurvey

class TestAnonymousSurvey(unittest.TestCase):
    def test_store_single_response(self):
        """测试单个答案能否会被妥善地存储"""
        question = "What language did you first learn to speak?"
        my_survey = AnonymousSurvey(question)
        my_survey.store_response("English")
        self.assertIn("English",my_survey.responses)#测试是否在列表中
unittest.main()
```



### 方法`setUp()`

`unittest.TestCase `类包含方法`setUp()` ，让我们只需创建这些对象一次，并在每个测试方法中使用它们

如果你在`TestCase `类中包含了方法`setUp()` ，Python将先运行它，再运行各个以test_打头的方法。这样，在你编写

的每个测试方法中都可使用在方法`setUp()` 中创建的对象了

从上述表述中推断: `test.main()`是寻找类中的test打头函数进行运行

```PY
def setUp(self):
""" 创建一个调查对象和一组答案，供使用的测试方法使用"""
	question = "What language did you first learn to speak?" 
	self.my_survey = AnonymousSurvey(question) 	
	self.responses = ['English', 'Spanish', 'Mandarin']	
```

方法setUp() 做了两件事情：创建一个调查对象；创建一个答案列表。存储这两样东西的变量名包含前缀self （即存储在属性中），因此可在这个类的任何地方使用。这让两个测试方法都更简单，因为它们都不用创建调查对象和答案



### 项目：使用pygame开发一个游戏

##### 窗口界面初始化

在游戏《外星人入侵》中，玩家控制着一艘最初出现在屏幕底部中央的飞船。玩家可以使用箭头键左右移动飞船，还可使用空格键进行射击。游戏开始时，一群外星人出现在天空中，他们在屏幕中向下移动。玩家的任务是射杀这些外星人。玩家将所有外星人都消灭干净后，将出现一群新的外星人，他们移动的速度更快。只要有外星人撞到了玩家的飞船或到达了屏幕底部，玩家就损失一艘飞船。玩家损失三艘飞船后，游戏结束

```shel
pip install pygame
```

​	首先，我们创建一个空的Pygame窗口。使用Pygame编写的游戏的基本结构如下：

```py
import sys #use it to exit the game when the player quits
import pygame
def run_game():
    # Initialize game and create a screen object.
    pygame.init()
    screen = pygame.display.set_mode((1200, 800))
    pygame.display.set_caption("Alien Invasion")
    #bg_color = (230, 230, 230)
    # Start the main loop for the game.
    while True:
        # Watch for keyboard and mouse events.
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                sys.exit()
    #   screen.fill(bg_color)
        pygame.display.flip()
run_game()
```

的代码行`pygame.init()` 初始化背景设置，让`Pygame`能够正确地工作。

用`pygame.display.set_mode()` 来创建一个名为`screen` 的显示窗口，这个游戏的所有图形元素都将在其中绘制。

其中实参(1200, 800) 是一个元组，指定了游戏窗口的尺寸。

对象screen 是一个surface。在`Pygame`中，surface是屏幕的一部分，用于显示游戏元素。在这个游戏中，每个元素（如外星人或飞船）都是一个surface。`display.set_mode()`返回的surface表示整个游戏窗口。我们激活游戏的动画循环后，每经过一次循环都将自动重绘这个surface。

```py
ps:
"""刚刚出现了显示 no modul named pygame的错误，自己检查又发现libs文件下存在pygame文件夹
而后发现是编译时是用了Microsoft Store下载的python 而不是自己下载在G盘的（pygame在G盘）"""
```

##### 设置setting

每次给游戏添加新功能时，通常也将引入一些新设置。下面来编写一个名为settings 的模块，其中包含一个名为Settings 的类，用于将所有设置存储在一个地方，以免在代码中到处添加设置。这样，我们就能传递一个设置对象，而不是众多不同的设置。另外，这让函数调用更简单，且在项目增大时修改游戏的外观更容易：要修改游戏，只需修改`settings.py`中的一些值，而无需查找散布在文件中的不同设置。下面是最初的Settings 类

```py
class Setting():
    
    def __init__(self):
        #initialize the game's settings
        self.screen_width = 1200
        self.screen_height = 800
        self.bg_color = (230, 230, 230)
```

##### 添加飞船图形

```py
import pygame
class Ship():
    """A class to manage the ship."""
    def __init__(self,screen):
        self.screen = screen
        # Load the ship image and get its rect.
        self.image = pygame.image.load('images/ship.bmp')
        self.rect = self.image.get_rect()
        self.screen_rect = screen.get_rect()
        # Start each new ship at the bottom center of the screen.
        self.rect.centerx = self.screen_rect.centerx
        self.rect.bottom = self.screen_rect.bottom
    def blitme(self):
        """Draw the ship at its current location."""
        self.screen.blit(self.image, self.rect)

```

Ship 的方法`__init__()` 接受两个参数：引用self 和screen ，其中后者指定了要将飞船绘制到什么地方

为加载图像，我们调用了`pygame.image.load()`

我们使用`get_rect()` 获取相应surface的属性`rect`(矩形)

要将游戏元素居中，可设置相应`rect` 对象的属性center 、`centerx` 或`centery` 。要让游戏元素与屏幕边缘对齐，可使用属性top 、bottom 、left 或right ；要调整游戏元素的水平或垂直位置，可使用属性x 和y ，它们分别是相应矩形左上角的 *x* 和 *y* 坐标。

![ship](C:\Users\15129\Pictures\Saved Pictures\ship.bmp)



##### 重构：game_functions模块

在大型项目中，经常需要在添加新代码前重构既有代码。重构旨在简化既有代码的结构，使其更容易扩展。在本节中，我们将创建一个名为game_functions 的新模块，它将存储大量让游戏《外星人入侵》运行的函数。通过创建模块game_functions ，可避`alien_invasion.py`太长，并使其逻辑更容易理解

###### 函数check_events()

我们将首先把管理事件的代码移到一个名为check_events() 的函数中，以简化run_game() 并隔离事件管理循环。通过隔离事件循环，可将事件管理与游戏的其他方面（如更新屏幕）分离。将check_events() 放在一个名为game_functions 的模块中：

```py
def check_events():
    """Respond to keypresses and mouse events."""
    for event in  pygame.event.get():
        if event.type == pygame.QUIT:
            sys.exit()
```

###### 函数update_screen()

```py
def update_screen(ai_setting, screen, ship):
    """Update images on the screen and flip to the new screen."""
    # Redraw the screen during each pass through the loop.        
    screen.fill(ai_setting.bg_color)
    ship.blitme()
    pygame.display.flip()#flip快速翻动
```

##### 驾驶飞船操作

###### 相应开关

每当用户按键时，都将在Pygame中注册一个事件。事件都是通过方法pygame.event.get() 获取的，因此在函数check_events() 中，我们需要指定要检查哪些类型的事件。每次按键都被注册为一个KEYDOWN 事件

检测到KEYDOWN 事件时，我们需要检查按下的是否是特定的键。例如，如果按下的是右箭头键，我们就增大飞船的rect.centerx 值，将飞船向右移动

```py
def check_events(ship):
    """Respond to keypresses and mouse events."""
    for event in  pygame.event.get():
        if event.type == pygame.QUIT:
            sys.exit()
        elif  event.type == pygame.KEYDOWN:
            if event.key == pygame.K_RIGHT:
                # Move the ship to the right.
                ship.rect.centerx += 10
            elif event.key == pygame.K_LEFT:
                # Move the ship to the left.
                ship.rect.centerx -= 10
```

###### 允许不断移动

in the file named `ship.py`

```py
#引入移动标志
	self.moving_right=False
	self.moving_left=False
def updata(self):
	if self.moving_right:
		self.rect.centerx+=1.5
	self if self.moving_left:
		self.rect.centerx-=1.5
```

in the file named `game_functions.py`

```py
def check_events(ship):
    """Respond to keypresses and mouse events."""
    for event in  pygame.event.get():
        if event.type == pygame.QUIT:
            sys.exit()
        elif  event.type == pygame.KEYDOWN:
            if event.key == pygame.K_RIGHT:
                ship.moving_right = True             
            elif event.key == pygame.K_LEFT:
                ship.moving_left = True
        elif  event.type == pygame.KEYUP:
            if event.key == pygame.K_RIGHT:
                ship.moving_right = False             
            elif event.key == pygame.K_LEFT:
                ship.moving_left = False
```

type是键位的种类 key 是具体的键位

###### 调整移动速度

###### 限制飞船活动范围

###### 重构check_events



##### 子弹类

本类保存所有的关于子弹的操作

###### 构建类

```py
#话不多说，直接上代码
import pygame
from pygame.sprite import Sprite#Sprite类可以将游戏中相关的元素编组，进而同时操作编组中的所有元素
from setting import Setting
class Bullet(Sprite):
    """A class to manage bullets fired from the ship"""
    def __init__(self,ai_setting,screen,ship)
        super(Bullet,self).__init__()#super()是一个特殊函数，帮助python将父类和子类关联起来
        self.screen = screen

        #create a bullet rect at (0,0) and then set correct position
        self.rect = pygame.Rect(0,0,ai_setting.bullet_width,ai_setting.bullet_height)
        #仅仅是初始化子弹，位置不重要，怎么方便怎么来
        self.rect.centerx = ship.rect.centerx
        self.rect.top = ship.rect.top-3

        #store the bullet's position as a decimal value
        self.y = float(self.rect.y)

        self.color = ai_setting.bullet_color
        self.speed_factor = ai_setting.bullet_speed_factor
        
        def update(self):
        """Move the bullet up the screen"""
        #update the decimal position of the bullet
        self.y -= self.speed_factor
        #update the rect position
        self.rect.y = self.y
    def draw_bullet(self):
        """Draw the bullet to the screen"""
        pygame.draw.rect(self.screen,self.color,self.rect)
```

Bullet 类继承了我们从模块`pygame.sprite `中导入的Sprite 类。通过使用精灵，可将游戏中相关的元素编组，进而同时操作编组中的所有元素。为创建子弹实例，需要向`__init__() `传递`ai_settings` 、`screen` 和`ship` 实例，还调用了`super() `来继承`Sprite `。

注意： 码`super(Bullet, self).__init__()` 使用了Python 2.7语法 而python 3 可以写成  `super().__init()__`

子弹元素并非基于图像的，因此我们必须使用`pygame.Rect()` 类从空白开始创建一个矩形。创建这个类的实例时，必须提供矩形左上角的

*x* 坐标和 *y* 坐标，还有矩形的宽度和高度，后续再调整子弹的位置

之后的操作就是调整其位置了

###### 在主函数中使用

定义Bullet 类和必要的设置后，就可以编写代码了，在玩家每次按空格键时都射出一发子弹。首先，我们将在`alien_invasion.py`中创建一个编组（group），用于存储所有有效的子弹，以便能够管理发射出去的所有子弹。这个编组将是`pygame.sprite.Group` 类的一个实例



```py
from pygame.sprite 
bullets=Group()

while True:
        # Watch for keyboard and mouse events.
        gf.check_events(ship)
        #通过标志判断是否移动并执行
        ship.update()
        bullets.update()
        # Redraw the screen during each pass through the loop.        
        gf.update_screen(ai_setting, screen, ship)
run_game() 	
```

我们导入了pygame.sprite 中的Group 类，我们在while之前创建了一个Group 实例，并将其命名为bullets 。这个编组是在while 循环外面创建的，这样就无需每次运行该循环时都创建一个新的子弹编组。

补充一句：

学游戏这个项目的时候记笔记实在不方便，如果不记得了就看书去



### 数据可视化

#### 生成数据

首先下载`matplotlib`

```shell
#cmd输入
pip install matplotlib
```

下面来使用`matplotlib`绘制一个简单的折线图，再对其进行定制，以实现信息更丰富的数据可视化。我们将使用平方数序列1、4、9、16和25来绘制这个图表

`mpl_squares`

```py
import matplotlib.pyplot as plt
squares = [1, 4, 9, 16, 25]
plt.plot(squares)
plt.show()#打开matplotlib查看器，并显示绘制的图形
```

导入了模块`pyplot` ，并给它指定了别名`plt` ，以免反复输入`pyplot`.  模块pyplot 包含很多用于生成图表的函数。

我们创建了一个列表，在其中存储了前述平方数，再将这个列表传递给函数plot() ，这个函数尝试根据这些数字绘制出有意义的图形





















