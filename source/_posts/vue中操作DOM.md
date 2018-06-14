---
title: vue中操作DOM
categories: VUE
tags: VUE
keywords: VUE
abbrlink: e7f1df7b
date: 2017-06-11 11:15:00
---

## 方案1
> 可以在mounted中挂载
```
mounted:function(){
        this.$nextTick(function(){         //this.$nextTick是在下次DOM更新循环结束时调用延迟回调函数。异步函数
            this.loadData();　　　　　　　　　　//DOM加载就绪，后调用loadData方法进行数据更新<br>　　　　　　　　//想要更新后的获取dom　　　　　　　　　　　　　　//此时若获取更新后dom数据将会报错，数据为undefined；
        })
    }

```

## 方案2

```
if(document.addEventListener){
                document.addEventListener('DOMMouseScroll',()=>{
                },false);
            }//W3C
            window.onmousewheel=document.onmousewheel=()=>{
                let top = document.getElementById("bottoms").scrollTop;
                console.log(top);
                this.$refs.add.style.marginTop = top + 10 + "px";
                // document.getElementById("rights").style.marginTop = top+"px";
                console.log(this.$refs.add.style);

            };//IE/Opera/Chrome

```