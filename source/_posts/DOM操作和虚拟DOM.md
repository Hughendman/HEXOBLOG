---
title: DOM操作和数据操作
categories: VUE
tags: VUE
keywords: VUE
abbrlink: 1e0e36a0
date: 2018-06-14 11:15:00
---

## js的DOM操作

> DOM操作会导致导致用户阻塞的重构(reflow)和重绘(repaint).在页面上的任何操作都是有代价的.reflow和repaint就是我们在改变页面或者说操作DOM时,会带来的两种后果.

> reflow意味着结构的改变,比如一堆元素堆叠,改变其中一个的宽高,那么相应的所有元素的位置都要改变.repaint意味着样式的改变比如div调整了背景色等,但是位置不变,只改变我们操作的元素.所以通常来看repaint的代价要远小于reflow,速度也更快.

## 虚拟DOM

> 对复杂的文档DOM结构，提供一种方便的工具，进行最小化地DOM操作

### 虚拟DOM快

* js很快
* DOM很慢

> 其实是由于每次生成virtual dom很快，diff生成patch也比较快，而在对DOM进行patch的时候，我能够根据Patch的内容，优化一部分DOM操作

[为什么虚拟dom更胜一筹](https://weibo.com/p/1001603915568079095157)