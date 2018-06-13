---
title: 大O表示法
categories: 算法
tags: 算法
abbrlink: fd43a581
date: 2018-06-14 11:15:00
---

## 作用

> 用来衡量算法的效率

> 通常是用资源，例如CPU(时间)占用，内存占用，硬盘占用，网络占用

## 理解

* O(1)

```
function o1(num){
    return ++num;
}
```
>  运行这个函数，执行时间是X ，不用管num值是多少，它的运行时间都是X，，因此它的复杂度是O(1)（常数）

* O(n)

```
function sequentialSearch(array,item){
    for(var i=0; i<array.length; i++){
        if( item == array[i]){
            return i;
        }
    }
    retuen -1;
}

```
> 这个函数的执行的总开销取决于数组元素的个数，而且和搜索的值有关，所以它的时间复杂度是O(n);

* O(n^2)

```
function swap (array,index1,index2){
    var temp = array[index1];
    array[index1] = array[index2];
    array[index2] = temp;
}

function bubbleSort (array){
    var length = array.lenth;
    var cost = 0;
    for (var i=0; i<length; i++){
        cost ++;
        for(var j=0; j<length-1; j++){
            cost ++;
            if(array[i] > array[j]){
                swap(array,i,j);
            }
        }
    }
}
```