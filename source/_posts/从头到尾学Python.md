---
title: 从头到尾学Python
categories:
  - python
tags:
  - python
keywords:
  - python
abbrlink: dcb6e033
date: 2018-11-10 16:15:00
---
### 修改字符串的大小写
#### title()

```
print('abcd'.title())

```

输出

```
Abcd
```

#### upper()

```
print('abcd'.upper())

```

输出

```
ABCD
```

#### lower()

```
print('ABCD'.lower())

```

输出

```
abcd

```

### 列表

```
name = ['name1','name2','name3']
```

#### 添加元素(append)

```
name.append('name4')

```
```
print(name[3]) 
# name4

```

#### 插入元素(insert)

```
name.insert(0,'name0')

```
```
print(name[0)
# name0

```
#### 删除元素(del)

```
del name[0]

```

#### 删除元素(pop)

```
name.pop()
# 删除最后一个元素
name.pop(0)
# 删除第一个元素
```

#### 根据值删除元素(remove)

```
name.remove("name0")

```

#### 组织列表
```
numbers = [1,4,2,8]
```
##### 永久排序（sort）
```
numbers.sort()
```


##### 临时排序（sorted）
临时排序不影响它们在列表中的原始排序顺序
```
numbers.sort()
```

##### 倒着打印（reverse）

```
numbers.reverse()

```

##### 列表长度（len）

```
len(numbers)
```

#### 索引-1返回最后一个元素

### 操作列表
```
nums = ['nums1','nums2','nums3']
```
#### 遍历列表
```
for info in nums:
    print(info)

```

输出：

```
nums1
nums2
nums3

```
#### 创建数字列表

##### 使用函数range()

```
for value in range(1,5):
    print(value)
```

输出：

```
1
2
3
4

```

##### 使用range()函数创建数字列表

```
print(list(range(1,6)))

```
输出：
```
[1, 2, 3, 4, 5]
```
指定步长

```
print(list(range(2,11,3)))

```

输出：

```
[2, 5, 8]

```

##### 对数字列表执行简单的统计计算

```
digits = [1,2,3,4,5,6,7,8,9,0]
min(digits)
max(digits)
sun(digits)
```

##### 列表解析
```
squares = [value**2 for value in range(1,11)]

```
输出：

```
[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]

```
#### 使用列表的一部分
```
name = ['name0','name1','name2','name3']

```
##### 切片
```
print(name[1:3])
print(name[1:])
print(name[:3])
print(name[-1:])
```
输出：

```
['name1', 'name2']
['name1', 'name2', 'name3']
['name0', 'name1', 'name2']
['name3']

```

##### 遍历切片
```
for info in name[-3:]:
    print(info)

```
输出：

```
name1
name2
name3

```

##### 复制列表

```
name_re = name[:]
print(name_re)
```

### 元组

#### 定义元组

```
b = (200.50)
print(b[0])

```

#### 遍历元组

```
b = (200.50)
for info in b:
    print(info)
```

#### 修改元组变量
不能修改元组的元素但是可以重新定义
```
b = (200.50)
b = (300.50)

```

### 条件语句

注：检测是否相等时考虑大小写

```
相等：==
不相等：!=
多个条件（并且）： and
多个条件（或者）： or
特定值在列表： 'name1' (not) in name
```
#### if语句

```
if True:
    print(1)
elif True:
    print(2)
else:
    print(3)

```

### 字典

```
alien_0 = {'color':'red','points':5}
print(alien_0['color'])
del alien_0["color"]
print(alien_0)
```

#### 遍历字典（items(),keys(),values()）

```
for key,value in alien_0.items():
    print(key,value)

```

输出：
```
points 5

```
#### 按照顺序遍历所有键

```
for key in sorted(alien_0.keys()):
    print(key)

```

输出：

```
color
points

```

### 用户输入和while循环

#### input()

```
message = input("Tell me something, and I will repeat it back to you")
print(message)

```

#### int() str() 类型转换

#### 求模运算符

```
4%5
# 输出4
4%2
# 输出0
4%3
# 输出1
```

#### while循环
```
num = 1
while num <=5:
    print(num)
    num+=1

```
#### break退出循环

#### continue返回循环开头

### 函数

```
def cats():
    print("hello")

```

调用：

```
cats()
```

#### 实参和形参

```
def greet_user(username):
    print("Hello "+username.title()+"!")

greet_user("yinxs")

```

在上面的代码中username是一个形参，而greet_user("yinxs")中的yinxs是一个实参

#### 传递实参

##### 位置实参
关联方式是基于实参的顺序，这种关联方式叫位置实参

```
def greet_user(username,age):
    print("Hello "+username.title()+"! I am " + str(age))

greet_user("yinxs",25)

```

##### 关键字实参

关键字实参是传递给函数的名称-值对。

```

def greet_user(username,age):
    print("Hello "+username.title()+"! I am " + str(age))

greet_user(age = 25,username = "yinxs")

```

可以设置默认值

```
def greet_user(username,age=25):
    print("Hello "+username.title()+"! I am " + str(age))

greet_user(username="yinxs")

```
##### 返回值

```
def greet_user(username,age=23):
    return username

print(greet_user(username="yinxs"))

```

##### 可选实参

```
def get_formatted_name(first_name,last_name,middle_name=''):
    if middle_name:
        full_name = first_name + " " + middle_name + " " + last_name
    else:
        full_name = first_name + " " + last_name
    return  full_name
print(get_formatted_name("jimi","Li"))
print(get_formatted_name("jimi","ho","Li"))

```

#### 传递列表

```
def greet_users(names):
    for info in names:
        print(info)
greet_users(["yinxs","psw","cxy"])

```

#### 将函数存储在模块中

##### 导入整个模块
要让函数是可导入的，得先创建模块
greet_users.py
```
def names(infos):
    for info in infos:
        print(info)

```

在另一个文件中

```
import greet_users

greet_users.names(["yinxs","psw","cxy"])
```

注意： 如果是用的pyCharm记得设置根目录在setting-source中

##### 导入特定的函数

```
from greet_users import names

names(["yinxs","psw","cxy"])

```

##### 使用as给模块指定别名

```
import greet_users as n

n.names(["yinxs","psw","cxy"])

```

##### 导入模块中所有函数

```
from greet_users import *

names(["yinxs","psw","cxy"])


```

#### 函数编写指南

* 形参指定默认值时，两边不能有控格。
* 如果包含多个函数，可使用两个空行将相邻的函数分开
* 代码行长度不要超过79个字符

### 类

#### 创建和使用类

##### 创建Dog类

根据Dog类创建的每个实例都将存储名字和年龄并赋予小哥蹲下和打滚的能力

```

class Dog():


    def __init__(self,name,age):
        self.name = name
        self.age = age


    def sit(self):
        print(self.name.title() + " is now sitting")


    def roll_over(self):
        print(self.name.title() + " rolled over!")

```

方法__init__()：
> 类中的函数称为方法。__init__()是一个特殊的方法，每当根据Dog类创建实例时，Python都会自动运行它。在这个方法的名称中，开头和末尾各有两个下划线，这是一种约定，旨在避免Python默认方法与普通方法发生名称冲突。在形参中self必不可少，位于开头，它是一个指向实例本身的引用。

##### 根据类创建实例

``` 
my_dog = Dog('Bai',5)
print(my_dog.name)
print(my_dog.age)
print(my_dog.sit())
print(my_dog.roll_over())

```

前面两个print中的是访问属性，后面两个print中的是调用方法


#### 使用类和实例

##### 指定默认值
```

class Dog():


    def __init__(self,name,age):
        self.name = name
        self.age = age
        self.race = "dog"

    def sit(self):
        print(self.name.title() + " is now sitting")


    def roll_over(self):
        print(self.name.title() + " rolled over!")

```
self.race = "dog" 为默认值


##### 修改默认值

直接修改

```
my_dog = Dog('Bai',5,'Teddy')
```

通过方法修改

```

class Dog():


    def __init__(self,name,age):
        self.name = name
        self.age = age
        self.race = "dog"

    def sit(self):
        print(self.name.title() + " is now sitting")


    def roll_over(self):
        print(self.name.title() + " rolled over!")
    
    
    def change_race(self,race):
        self.race = race

```

```
my_dog.change_race("Teddy")

```

#### 继承

一个类继承另一个类

##### 子类的方法__init__()


```
class Car():


    def  __init__(self,make,model,year):
        self.make = make
        self.model = model
        self.year = year
        self.odometer_reading = 0

    def get_derscriptive_name(self):
        long_name = str(self.year)+ " " + self.make + " " + self.model
        return  long_name.title()

    def read_odometer(self):
        print("This cat has " + str(self.odometer_reading) + " miles on it.")

    def update_odometer(self,mileage):
        if mileage >= self.odometer_reading:
            self.odometer_reading = mileage
        else:
            print("You can't roll back an odometer!")

    def increment_odometer(self,miles):
        self.odometer_reading += miles

class ElectricCar(Car):

    def __init__(self,make,model,year):
        super().__init__(make,model,year)


my_tesla = ElectricCar("tesla","model s",2018)
print(my_tesla.get_derscriptive_name())

```

###### python2.7中的继承

```
class Car(object):
    def __init__(self,make,model,year):
        --snip--
        
class ElectricCar(Car):
    def __init__(self,make,model,year):
        super(ElectricCar,self).__init__(make,model,year)
        --snip--
```

###### 将实例用作属性

```
class Car():
    def __init__(self, make, model, year):
        self.make = make
        self.model = model
        self.year = year
        self.odometer_reading = 0

    def get_derscriptive_name(self):
        long_name = str(self.year) + " " + self.make + " " + self.model
        return long_name.title()

    def read_odometer(self):
        print("This cat has " + str(self.odometer_reading) + " miles on it.")

    def update_odometer(self, mileage):
        if mileage >= self.odometer_reading:
            self.odometer_reading = mileage
        else:
            print("You can't roll back an odometer!")

    def increment_odometer(self, miles):
        self.odometer_reading += miles

class Battery():

    def __init__(self,battery_size=70):
        self.battery_size = battery_size

    def descript_battery(self):
        print("This car has a " + str(self.battery_size) + "-kwh battery.")

class ElectricCar(Car):
    def __init__(self, make, model, year):
        super().__init__( make, model, year)
        self.battery = Battery()


my_tesla = ElectricCar("tesla","model s",2018)

print(my_tesla.get_derscriptive_name())
my_tesla.battery.descript_battery()

```

#### 导入类
car.py
```
class Car():
    def __init__(self, make, model, year):
        self.make = make
        self.model = model
        self.year = year
        self.odometer_reading = 0

    def get_derscriptive_name(self):
        long_name = str(self.year) + " " + self.make + " " + self.model
        return long_name.title()

    def read_odometer(self):
        print("This cat has " + str(self.odometer_reading) + " miles on it.")

    def update_odometer(self, mileage):
        if mileage >= self.odometer_reading:
            self.odometer_reading = mileage
        else:
            print("You can't roll back an odometer!")

    def increment_odometer(self, miles):
        self.odometer_reading += miles

```

在文件中导入：

```
from car import Car

my_new_car = Car("audi","a4",2020)
print(my_new_car.get_derscriptive_name())

my_new_car.odometer_reading = 23
my_new_car.read_odometer()

```

##### 导入多个类

car.py
```
class Car():
    def __init__(self, make, model, year):
        self.make = make
        self.model = model
        self.year = year
        self.odometer_reading = 0

    def get_derscriptive_name(self):
        long_name = str(self.year) + " " + self.make + " " + self.model
        return long_name.title()

    def read_odometer(self):
        print("This cat has " + str(self.odometer_reading) + " miles on it.")

    def update_odometer(self, mileage):
        if mileage >= self.odometer_reading:
            self.odometer_reading = mileage
        else:
            print("You can't roll back an odometer!")

    def increment_odometer(self, miles):
        self.odometer_reading += miles

class Battery():

    def __init__(self,battery_size=70):
        self.battery_size = battery_size

    def descript_battery(self):
        print("This car has a " + str(self.battery_size) + "-kwh battery.")

class ElectricCar(Car):
    def __init__(self, make, model, year):
        super().__init__( make, model, year)
        self.battery = Battery()

```

导入:

```
from car import Car,ElectricCar

my_beetle = Car("volkswagen","beetle",2019)
print(my_beetle.get_derscriptive_name())

my_tesla = ElectricCar("tesla","roadster",2020)
print(my_tesla.get_derscriptive_name())

```

##### 导入整个模块

```
import car

```

##### 导入模块中的所有类

```
form car import *

```

### 异常

#### 使用try-except代码块

```
try:
    print(5/0)
except ZeroDivisionError:
    print("You can't divide by zero!")

```
失败时无提示
```
try:
    print(5/0)
except ZeroDivisionError:
    pass

```