---
title: Lua语言简介
categories: node
tags:
  - node
  - 理论
keywords: node
abbrlink: 1f21691d
date: 2018-06-09 11:15:00
---
## 简介 

Lua是一门小巧的脚本语言，由巴西里约热内卢天主教大学的Roberto Ierusalimschy等人于1993年开发，在现代企业开发中Lua通常被作为胶水语言，在游戏领域应用尤为频繁。

### Lua中的数据类型

Lua中定义以下几种数据类型

##### 1、Nil
nil是一个特殊的数据类型，它只有一个值，就是nil，他的作用就是为了区别其他的值

##### 2、Boolean
和其他语言一样，布尔值只用true和false两个值

##### 3、Number
用来表示实数的类型，包含整数和浮点数，Lua中number类型可以表示32位整数

##### 4、String
string类型用来表示一个字符串，Lua中的字符串是不可变类型，此外Lua也存在数字和字符串之间的隐式转换
```

print('10'+1)  //11

```

##### 5、table
table类型实现了一种特殊的数组，特殊之处在于该数字的索引方式，传统的数组索引是通过数组下标来实现的，二table不仅能以整数来索引，还可以使用字符串和其他类型的值进行索引。
table没有固定的大小，可以在里面放入任意数量的元素，下面的例子简单展示了table的用法

```
a = {} //声明一个table
a[1] = 10
a["name"] = "yinxs"

print(a[1])//10
print(a["name"])//yinxs

```
### Lua定义一个函数

Lua没有使用大括号来规定函数的作用域，而是使用end关键字来作为结束的标记

eg：

```
function add(a)
    local sun = 0
    for i,v in ipairs(a) do
        sum = sum + v
    end
    return sum
end

```
es6中有关于函数的新特性和Lua中的函数有一些相似之处：
* 多重返回值
* 变长参数

一个Lua函数可以返回多个值，值需要在return后面列出需要返回的值即可，用逗号隔开

eg：

```
funcion foo()
    return "a","b"
end

```
尝试使用print语句来打印foo函数的执行结果，会输出“a”，“b”。
如果使用表达书的形式来调用foo函数，会依照解构赋值额原则来赋值

```
x = foo() //x = "a","b"被丢掉
x,y = foo() //x = "a",y = "b"
x,y,z = foo() //x = "a",y = "b",z = nil

```
可变参数，这个特性即为es6中的spread运算符，函数可以接收任意长度的参数。

```

function add(...)
local s = 0
    for i,v in ipirs {...} do
        s = s+v
    end
return s
end
print(add(3,4,10,25,12) //54

```

### Lua中的协程

Lua设计之初就提供了对协程的支持，跟同时期的其他编程语言相比无疑是超前的，Lua将所有协程相关的函数放在一个名为coroutine的table中，一个协程其实就是一个特殊线程，它可以由用户控制状态的切换。

##### 1、coroutine.create()
创建一个coroutine 并返回，参数是一个函数。
eg:

```
function log(i)
    print(i);
    end
    
co = coroutine.create(log)


```
我们声明了一个log方法，并用其作为参数创建一个协程，这代表print方法的执行可以被用户终端或恢复。

##### 2、coroutine.resume()
重启coroutine，和create配合使用
上面的diamante新建了一个协程之后并不会直接运行，而是要靠resume方法来启动。

```
coroutine.resume(c0,1) --1

```

##### 3、coroutine.yield()
将coroutine设置为挂起状态，可以由resume来恢复执行

```
function log(i)
 for i=1,10 do
    print(i)
    coroutine.yielld()
 end
end
co = coroutine.create(log)

coroutine.resume(co) //1

coroutine.resume(co) //2

coroutine.resume(co) //3

print(coroutine.status(co) //suspended

print(coroutine.running()) // thread:0x7fd637c02940 true

```

coroutine.status()为查看coroutine的状态

一个协程可以有三种不同的状态

* suspended
* running
* dead

当创建一个协程后，协程默认处于suspended状态，使用yield挂起后状态同样转换为suspended。

##### 4、coroutine.running()
返回当前协程的线程号