---
title: Python3基础语法
categories: python
tags: python
keywords: python
abbrlink: e5ac5920
date: 2018-06-11 11:15:00
---

## 条件判断


```
age = 3
if age >= 18:
    print('adult')
elif age >= 6:
    print('teenager')
else:
    print('kid')
    
```
## 循环语句

```
for x in [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]:
    sum = sum + x
print(sum)

```

```
sum = 0
n = 99
while n > 0:
    sum = sum + n
    n = n - 2
print(sum)

```

```
n = 1
while n <= 100:
    if n > 10: # 当n = 11时，条件满足，执行break语句
        break # break语句会结束当前循环
    print(n)
    n = n + 1
print('END')

```

```
n = 0
while n < 10:
    n = n + 1
    if n % 2 == 0: # 如果n是偶数，执行continue语句
        continue # continue语句会直接继续下一轮循环，后续的print()语句不会执行
    print(n)

```

## dict(字典)

```
d = {'Michael': 95, 'Bob': 75, 'Tracy': 85}
d['Michael']

```
##### get()

```
d.get("Michael");
#key不存在返回None，或者自己指定的value 

```
##### pop(key)

```
d.pop('Bob')

# 要删除一个key，用pop(key)方法，对应的value也会从dict中删除：

```

> 请务必注意，dict内部存放的顺序和key放入的顺序是没有关系的。

> 和list比较，dict有以下几个特点：

* 查找和插入的速度极快，不会随着key的增加而变慢；
* 需要占用大量的内存，内存浪费多。

> 而list相反：

* 查找和插入的时间随着元素的增加而增加；
* 占用空间小，浪费内存很少。

> 所以，dict是用空间来换取时间的一种方法。

> dict可以用在需要高速查找的很多地方，在Python代码中几乎无处不在，正确使用dict非常重要，需要牢记的第一条就是dict的key必须是不可变对象。

> 这是因为dict根据key来计算value的存储位置，如果每次计算相同的key得出的结果不同，那dict内部就完全混乱了。这个通过key计算位置的算法称为哈希算法（Hash）。

> 要保证hash的正确性，作为key的对象就不能变。在Python中，字符串、整数等都是不可变的，因此，可以放心地作为key。而list是可变的，就不能作为key

## set

> set和dict类似，也是一组key的集合，但不存储value。由于key不能重复，所以，在set中，没有重复的key。

```
>>> s = set([1, 2, 3])
>>> s
{1, 2, 3}

```
##### add(key)

> 通过add(key)方法可以添加元素到set中，可以重复添加，但不会有效果：

```
>>> s.add(4)
>>> s
{1, 2, 3, 4}
>>> s.add(4)
>>> s
{1, 2, 3, 4}

```

##### remove(key)

> 通过remove(key)方法可以删除元素：

```
>>> s.remove(4)
>>> s
{1, 2, 3}

```

> set可以看成数学意义上的无序和无重复元素的集合，因此，两个set可以做数学意义上的交集、并集等操作：

```
>>> s1 = set([1, 2, 3])
>>> s2 = set([2, 3, 4])
>>> s1 & s2
{2, 3}
>>> s1 | s2
{1, 2, 3, 4}

```

> set和dict的唯一区别仅在于没有存储对应的value，但是，set的原理和dict一样，所以，同样不可以放入可变对象，因为无法判断两个可变对象是否相等，也就无法保证set内部“不会有重复元素”。试试把list放入set，看看是否会报错。


## 数据类型转换

##### int()函数可以把其他数据类型转换为整数

```
>>> int('123')
123
>>> int(12.34)
12
>>> float('12.34')
12.34
>>> str(1.23)
'1.23'
>>> str(100)
'100'
>>> bool(1)
True
>>> bool('')
False

```



