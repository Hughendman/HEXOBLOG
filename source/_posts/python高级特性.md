---
title: python高级特性
date: 2018-06-11 11:15:00
categories : python
tags: python
keywords : python
---


## 切片

```
L = ['Michael', 'Sarah', 'Tracy', 'Bob', 'Jack']
# 取前三个元素

r = []
n = 3
for i in range(n):
    r.append(L[i])
 
# >>> r
# ['Michael', 'Sarah', 'Tracy']
```
> 也可以用一行代码实现

```
>>> L[0:3]
['Michael', 'Sarah', 'Tracy']

# 0 也可以省略
>>> L[:3]
['Michael', 'Sarah', 'Tracy']

# 也可以从索引1开始
>>> L[1:3]
['Sarah', 'Tracy']

# 也可以倒着取
>>> L[-2:]
['Bob', 'Jack']
>>> L[-2:-1]
['Bob']

# 每五个取一个
>>> L[::5]
[0, 5, 10, 15, 20, 25, 30, 35, 40, 45, 50, 55, 60, 65, 70, 75, 80, 85, 90, 95]

# 也可以隔着取，每两个取一个
>>> L[:10:2]
[0, 2, 4, 6, 8]
```

> 注：字符串也可以看成一种list

## 迭代

```
>>> for x, y in [(1, 1), (2, 4), (3, 9)]:
...     print(x, y)
...
1 1
2 4
3 9

```

## 列表生成式

```
>>> list(range(1, 11))
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# 生成[1x1, 2x2, 3x3, ..., 10x10]
>>> L = []
>>> for x in range(1, 11):
...    L.append(x * x)
...
>>> L
[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
# 上面的简写
>>> [x * x for x in range(1, 11)]
[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
#还可以这么写
>>> [m + n for m in 'ABC' for n in 'XYZ']
['AX', 'AY', 'AZ', 'BX', 'BY', 'BZ', 'CX', 'CY', 'CZ']

```

## 生成器(generator)

```
>>> L = [x * x for x in range(10)]
>>> L
[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
>>> g = (x * x for x in range(10))
>>> g
<generator object <genexpr> at 0x1022ef630>

# 可以通过next()函数获得generator的下一个返回值

```

> generator保存的是算法，每次调用next(g)，就计算出g的下一个元素的值，直到计算到最后一个元素，没有更多的元素时，抛出StopIteration的错误。

> 当然，上面这种不断调用next(g)实在是太变态了，正确的方法是使用for循环，因为generator也是可迭代对象：

```
>>> g = (x * x for x in range(10))
>>> for n in g:
...     print(n)
... 
0
1
4
9
16
25
36
49
64
81

```

# 断点》》》》》》》》》》》》》》》》》
  



