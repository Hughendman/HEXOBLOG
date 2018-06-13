---
title: svg
categories: 数据可视化
tags: svg
keywords: 数据可视化
abbrlink: '3452590'
date: 2018-06-13 11:15:00
---

## svg

> SVG 意为可缩放矢量图形（Scalable Vector Graphics）。

> SVG 使用 XML 格式定义图像。

> SVG 是使用 XML 来描述二维图形和绘图程序的语言。

* SVG 指可伸缩矢量图形 (Scalable Vector Graphics)
* SVG 用来定义用于网络的基于矢量的图形
* SVG 使用 XML 格式定义图形
* SVG 图像在放大或改变尺寸的情况下其图形质量不会有所损失
* SVG 是万维网联盟的标准
* SVG 与诸如 DOM 和 XSL 之类的 W3C 标准是一个整体
* 
## 在html中使用SVG

####  方法一：<embed>

```
<embed src="rect.svg" width="300" height="100" 
type="image/svg+xml"
pluginspage="http://www.adobe.com/svg/viewer/install/" />

```
> <embed> 标签被所有主流的浏览器支持，并允许使用脚本。

> 当在 HTML 页面中嵌入 SVG 时使用 <embed> 标签是 Adobe SVG Viewer 推荐的方法！然而，如果需要创建合法的 XHTML，就不能使用 <embed>。任何 HTML 规范中都没有 <embed> 标签。

#### 方法二：<object>

```
<object data="rect.svg" width="300" height="100" 
type="image/svg+xml"
codebase="http://www.adobe.com/svg/viewer/install/" />
```
> <object> 标签是 HTML 4 的标准标签，被所有较新的浏览器支持。它的缺点是不允许使用脚本。

> 假如您安装了最新版本的 Adobe SVG Viewer，那么当使用 <object> 标签时 SVG 文件无法工作（至少不能在 IE 中工作）！

#### 方法三：使用 <iframe> 标签

> <iframe> 标签可工作在大部分的浏览器中。

```
<iframe src="rect.svg" width="300" height="100">
</iframe>
```


