---
title: pandas使用
categories:
  - python
tags:
  - python
keywords:
  - python
abbrlink: 80bbe987
date: 2018-11-07 16:15:00
---
## 简介

 pandas 是基于NumPy 的一种工具，该工具是为了解决数据分析任务而创建的。
 
 
## 数据结构

### Series一维数组
Series：一维数组，与Numpy中的一维array类似。二者与Python基本的数据结构List也很相近，其区别是：List中的元素可以是不同的数据类型，而Array和Series中则只允许存储相同的数据类型，这样可以更有效的使用内存，提高运算效率。
Time- Series：以时间为索引的Series。
### DataFrame二维的表格型数据结构
DataFrame：二维的表格型数据结构。很多功能与R中的data.frame类似。可以将DataFrame理解为Series的容器
### Panel三维的数组
Panel ：三维的数组，可以理解为DataFrame的容器。

## 使用

### 安装

```
pip install pandas 

```

### 导入


#### series模块

```
from pandas import Series
```

#### 别名

```
import pandas as pd
```

### 案例

#### series模块

```
from pandas import Series

data = Series([1,2,3,4])
print(data)

```
输出

```
0    1
1    2
2    3
3    4
dtype: int64

```
##### index

```
print(data.index)
```
```
RangeIndex(start=0, stop=4, step=1)

```
##### values
```
print(data.values)
```

```
[1 2 3 4]

```

#### DataFrame

```
from pandas import DataFrame
data = DataFrame({"name": ['google', 'baidu', 'yahoo'], "tag": [1, 2, 3]})

print(data)

```
输出
```
     name  tag
0  google    1
1   baidu    2
2   yahoo    3

```
##### 排序

```
from pandas import DataFrame
data = DataFrame({"name": ['google', 'baidu', 'yahoo'], "tag": [1, 2, 3]},columns=['tag','name'])

print(data)

```
输出
```
   tag    name
0    1  google
1    2   baidu
2    3   yahoo

```

##### 自定义索引index
```
from pandas import DataFrame
data = DataFrame({"name": ['google', 'baidu', 'yahoo'], "tag": [1, 2, 3]},index=['a','b','c'])

print(data)

```
输出
```
     name  tag
a  google    1
b   baidu    2
c   yahoo    3
```

##### 字典套字典

```
from pandas import DataFrame
data = DataFrame({'human':{'first':'man','second':'woman'},'age':{'first':1,'second':2}})

print(data)

```
输出

```
        age  human
first     1    man
second    2  woman

```

## 其他

### python 读取csv文件

```
import pandas as pd

csv = pd.read_csv("sqllab_Hui Ping Mai Dian _20181106T084828.csv",header=None)
print(csv.header())
print(csv.tail())
```
#### 转为json

```
csv.to_json(orient='records')
```