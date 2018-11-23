---
title: less回顾
categories:
  - less
  - wehotel
tags:
  - less
  - wehotel
keywords: less
abbrlink: 41cce60d
date: 2018-11-12 16:15:00
---

##### 忘得一干二净，回顾一下


### 变量（Variables）

这些都是不言自明的:
```
@width: 10px;
@height: @width + 10px;

#header {
  width: @width;
  height: @height;
}

```

### 混合（Mixins）

mixin是一种将一组属性从一个规则集包含到另一个规则集的方法(“混合”)：

```
.bordered {
  border-top: dotted 1px black;
  border-bottom: solid 2px black;
}

#menu a {
  color: #111;
  .bordered();
}

.post a {
  color: red;
  .bordered();
}

```

### 嵌套（Nesting）

Less提供了使用嵌套而不是与级联结合使用嵌套的能力:
```
#header {
  color: black;
  .navigation {
    font-size: 12px;
  }
  .logo {
    width: 300px;
  }
}

.clearfix {
  display: block;
  zoom: 1;

  &:after {
    content: " ";
    display: block;
    font-size: 0;
    height: 0;
    clear: both;
    visibility: hidden;
  }
}

.component {
  width: 300px;
  @media (min-width: 768px) {
    width: 600px;
    @media  (min-resolution: 192dpi) {
      background-image: url(/img/retina2x.png);
    }
  }
  @media (min-width: 1280px) {
    width: 800px;
  }
}


```

### 运算（Operations）

```

// numbers are converted into the same units
@conversion-1: 5cm + 10mm; // result is 6cm
@conversion-2: 2 - 3cm - 5mm; // result is -1.5cm

// conversion is impossible
@incompatible-units: 2 + 5px - 3cm; // result is 4px

// example with variables
@base: 5%;
@filler: @base * 2; // result is 10%
@other: @base + @filler; // result is 15%

@base: 2cm * 3mm; // result is 6cm
background-color: #112244 + #111; // result is #223355

```

#### calc() 特例
为了与 CSS 保持兼容，calc() 并不对数学表达式进行计算，但是在嵌套函数中会计算变量和数学公式的值。
```
@var: 50vh/2;
width: calc(50% + (@var - 20px));  // result is calc(50% + (25vh - 20px))

```

### Escaping

```
@min768: ~"(min-width: 768px)";
.element {
  @media @min768 {
    font-size: 1.2rem;
  }
}

```

编译为：

```
@media (min-width: 768px) {
  .element {
    font-size: 1.2rem;
  }
}

```

### 函数（Functions）

Less 内置了多种函数用于转换颜色、处理字符串、算术运算等。这些函数在函数手册中有详细介绍。

函数的用法非常简单。下面这个例子将介绍如何利用 percentage 函数将 0.5 转换为 50%，将颜色饱和度增加 5%，以及颜色亮度降低 25% 并且色相值增加 8 等用法：

```
@base: #f04615;
@width: 0.5;

.class {
  width: percentage(@width); // returns `50%`
  color: saturate(@base, 5%);
  background-color: spin(lighten(@base, 25%), 8);
}

```
#### color

```
color("#aaa");

```

#### image-size
```
image-size("file.png");

```
#### image-width
```
image-width("file.png");

```

#### image-height

```
image-height("file.png");

```
#### if

```
@some: foo;

div {
    margin: if((2 > 1), 0, 3px);
    color:  if((iscolor(@some)), darken(@some, 10%), black);
}

```

### 名称空间和访问器(Namespaces and Accessors)

```
#bundle() {
  .button {
    display: block;
    border: 1px solid black;
    background-color: grey;
    &:hover {
      background-color: white;
    }
  }
  .tab { ... }
  .citation { ... }
}

```

使用：

```
#header a {
  color: orange;
  #bundle.button();  // can also be written as #bundle > .button
}

```

### Maps

```
#colors() {
  primary: blue;
  secondary: green;
}

.button {
  color: #colors[primary];
  border: 1px solid #colors[secondary];
}

```

### 作用域（Scope）

```
@var: red;

#page {
  @var: white;
  #header {
    color: @var; // white
  }
}

```

### 注释（Comments）

```
/* 一个块注释
 * style comment! */
@var: red;

// 这一行被注释掉了！
@var: white;

```

### 导入（Importing）

```
@import "library"; // library.less
@import "typo.css";


```