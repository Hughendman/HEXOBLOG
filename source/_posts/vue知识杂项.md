---
title: vue知识杂项
categories: VUE
tags: VUE
keywords: VUE
abbrlink: a3fa9686
date: 2018-07-06 16:15:00
---

## 快速构建项目

### 全局安装
```
npm i -g vue-cli
```

### 创建项目(项目名称叫muses)

```
vue init webpack muses
```

### 启动(默认的是80端)

```
npm run dev
```

### 详细讲解

> 与ng4 的cli 不同 ，vue的cli比较复杂 ，但是实际上只要一直回车对后面没有影响，注意最后一项要选择npm的那个选项

> build文件里面是一些操作文件,执行 npm run * 时执行的就是这里的文件

> config 文件是配置文件

> src 是资源文件，组件等都放在这里

> assets 资源文件，同ng4 类似，放的是公共的资源，例如图片

> 

### 打包

```
npm run build
```

### 注意

> 如果你是用的编辑器是webstorm 那么需要在setting中的Language选项里面的JavaScript设置为ECMAScript 6，这样才可以使用，如果还有问题，那么需要在script标签中标注type为es6




## 引入facvion

```

// build/webpack.dev.conf.js

new HtmlWebpackPlugin({
    filename: 'index.html',
    template: 'index.html',
    inject: true,
    favicon: path.resolve('favicon.ico')   // 加上这个
})
```

favicon.ico 放在根目录下


## 页面传参

### vuex

参考：[这篇文章不错](http://blog.csdn.net/github_26672553/article/details/53176778)

> vuex是一个为了vue开发的状态管理模式，它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化，

#### 安装

```
npm i vuex --save
```

#### 引入

```
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

const mutations = {...};
const actions = {...};
const state = {...};
 
Vuex.Store({
  state,
  actions,
  mutation
});

```
#### 使用

```

import {mapActions} from 'vuex'
 
//我是一个组件
export default {
  methods: mapActions([
    'actionName',
  ])
}

```

### query传参

query是拼接在url后面的参数

query:/home?id=1 ,/home?id=2 ,这里的id叫做query

#### 使用

```
 <router-link :to="{ name:'home',query: { id: 1 }}" >home</router-link>
 
 //或者
 
 this.$router.push({  name:'home',query: { id:  1 }});


```

#### 获取

```
this.$route.query.id

```

### params传参

params是路由的一部分，注意，设置了params，那么params就是路由的一部分，一旦没有传params，那么会导致跳转失败或者没有内容

params：/home/:id ，/home/1，/home/2 ,这里的id叫做params

#### 路由配置

params使用的时候需要配置路由

```

routes:[
    {
        path:'/home/:id',
        name:'home',
        component:Home
    }
]

```

#### 使用

```
 <router-link :to="{ name:'home',params: { id: 1}" >home</router-link>
 
 //或者
 
 this.$router.push({  name:'home',params: { id:1}});


```

#### 获取

```
this.$route.params.id

```
### 广播

在vue1.0中使用的是 $dispatch 和 $broadcast ，但是目前为止已经弃用。

在vue2.0中使用的是 $emit, $on, $off 来分发、监听、取消监听事件

#### 父传子

父：
```
<template>
  <div>
    父组件:
    <input type="text" v-model="name">
    <!-- child子组件 -->
    <child :inputName="name"></child>
  </div>
</template>
<script>
  export default {
    data () {
      return {
        name: ''
      }
    }
  }
</script>


```
子：
```
<template>
  <div>
    子组件:
    <span>{{inputName}}</span>
  </div>
</template>
<script>
  export default {
    // 接收父组件的值
    props: {
      inputName: String,
      required: true
    }
  }
</script>

```

#### 子传父

父：
```

<template>
  <div>
    父组件:
    <span>{{name}}</span>
    <br>
    <br>
    <!-- 引入子组件 定义一个on的方法监听子组件的状态-->
    <child v-on:childByValue="childByValue"></child>
  </div>
</template>
<script>
  import child from './child'
  export default {
    components: {
      child
    },
    data () {
      return {
        name: ''
      }
    },
    methods: {
      childByValue: function (childValue) {
        // childValue就是子组件传过来的值
        this.name = childValue
      }
    }
  }
</script>

```

子：
```
<template>
  <div>
    子组件:
    <span>{{childValue}}</span>
    <!-- 定义一个子组件传值的方法 -->
    <input type="button" value="点击触发" @click="childClick">
  </div>
</template>
<script>
  export default {
    data () {
      return {
        childValue: '我是子组件的数据'
      }
    },
    methods: {
      childClick () {
        // childByValue是在父组件on监听的方法
        // 第二个参数this.childValue是需要传的值
        this.$emit('childByValue', this.childValue)
      }
    }
  }
</script>

```

### 非父子传值

非父子组件传值，需要定义一个公共的实例文件bus.js来作为中间仓库来传值

bus.js文件

```
import Vue form 'vue'

export default new Vue()

```
组件1：

```
<template>
  <div>
    A组件:
    <span>{{elementValue}}</span>
    <input type="button" value="点击触发" @click="elementByValue">
  </div>
</template>
<script>
  // 引入公共的bus，来做为中间传达的工具
  import Bus from './bus.js'
  export default {
    data () {
      return {
        elementValue: 4
      }
    },
    methods: {
      elementByValue: function () {
        Bus.$emit('val', this.elementValue)
      }
    }
  }
</script>

```

组件2：

```
<template>
  <div>
    B组件:
    <input type="button" value="点击触发" @click="getData">
    <span>{{name}}</span>
  </div>
</template>
<script>
  import Bus from './bus.js'
  export default {
    data () {
      return {
        name: 0
      }
    },
    mounted: function () {
      var vm = this
      // 用$on事件来接收参数
      Bus.$on('val', (data) => {
        console.log(data)
        vm.name = data
      })
    },
    methods: {
      getData: function () {
        this.name++
      }
    }
  }
</script>

```

## 插件的使用

### element-ui

#### 安装
```
cnpm i element-ui --save
```

#### 引入

> 在main.js 中

```

import Vue from 'vue'
import ElementUI from 'element-ui'
import 'element-ui/lib/theme-chalk/index.css'
import App from './App.vue'

Vue.use(ElementUI)

new Vue({
  el: '#app',
  render: h => h(App)
})
```

#### [使用菜单导航](https://segmentfault.com/a/1190000007810151)

```

<el-menu :router="true" :default-active="$route.path" class="el-menu-demo" mode="horizontal" @select="handleSelect">
      <el-menu-item index="/table">表格</el-menu-item>
      <el-menu-item index="/form">表单</el-menu-item>
      <el-menu-item index="/graph">图形</el-menu-item>
    </el-menu>
```
> index里面添加路由，  router是使用路由模式为true

> 但是还会发现一个新的问题，它不默认选中了，所以这里要修改一下

```
<el-menu :router="true" :default-active="$route.path" class="el-menu-demo" mode="horizontal" @select="handleSelect">
      <el-menu-item index="/table">表格</el-menu-item>
      <el-menu-item index="/form">表单</el-menu-item>
      <el-menu-item index="/graph">图形</el-menu-item>
    </el-menu>
```
#### elementUI关于树状图的增删改查，局部刷新问题

[链接在这里](https://segmentfault.com/a/1190000011574698)


#### elementUI关于tree的使用

[getCheckedKeys不能获取父节点的key](https://segmentfault.com/q/1010000012345769/a-1020000012347029)

### 在vue中使用less

#### 安装

> cnpm install less less-loader --save

> 在webpack.base.config.js在loaders里面加上


```

{

test: /\.less$/,

loader: "style-loader!css-loader!less-loader",

},
```

```
<style scoped lang="less">
```

### vue中使用sass

#### 安装

```
npm install node-sass --save-dev
npm install sass-loader --save-dev

```

#### 配置

> 打开webpack.base.config.js在loaders里面加上  module -- rules

```
rules: [
      {
        test: /\.vue$/,
        loader: 'vue-loader',
        options: vueLoaderConfig
      },
      {
        test: /\.js$/,
        loader: 'babel-loader',
        include: [resolve('src'), resolve('test')]
      },
      {
        test: /\.(png|jpe?g|gif|svg)(\?.*)?$/,
        loader: 'url-loader',
        query: {
          limit: 10000,
          name: utils.assetsPath('img/[name].[hash:7].[ext]')
        }
      },
      {
        test: /\.scss$/,
        loaders: ["style", "css", "sass"]
      },//这里这里看这里，主要引入这一段
      {
        test: /\.(woff2?|eot|ttf|otf)(\?.*)?$/,
        loader: 'url-loader',
        query: {
          limit: 10000,
          name: utils.assetsPath('fonts/[name].[hash:7].[ext]')
        }
      }
    ]
  }

```
### vue中使用富文本编辑器

```
## 安装
> cnpm install vue-quill-editor --save

## 引入

> import { quillEditor } from 'vue-quill-editor'

```


### 在vue中使用Echarts（vue）

#### 安装

> cnpm install echarts --save

#### 引入

> import echarts from 'echarts'

> Vue.prototype.$echarts = echarts 

#### 使用

具体使用可以看我的[git代码](https://github.com/Hughendman/vue-echarts/blob/master/echarts/public/fronted/src/components/canvas/eChart_1.vue)

```
<template>
  <div>
    <h1>e-chart_1:南丁格尔图</h1>
    <div class="charts">
      <div class="myChart" :style="{width: '500px', height: '500px'}"></div>
      <div class="tip">
        <vue-markdown :ishljs = "true">{{msg}}</vue-markdown>
      </div>
    </div>
  </div>
</template>

<script>
    import VueMarkdown from 'vue-markdown'
    import {rose} from '../../assets/eCharts/rose';
    import rosemd from '../../assets/md/rose.md';
    export default {
      data(){
        return {
          msg:rosemd
        }
      },
      mounted(){
        this.drawLine();
      },
      methods: {
        drawLine(){
          // 基于准备好的dom，初始化echarts实例
          let myChart = this.$echarts.init(document.getElementsByClassName('myChart')[0]);
          // 绘制图表
          let option = rose();
          myChart.setOption(option);
        }
      },
      components: {
        VueMarkdown
      }
    }
</script>

<style scoped lang="less">
  .charts {
    padding: 20px;
    position: relative;
    .myChart {
      position: absolute;
      left: 0px;
    }
    .tip {
      position: absolute;
      right: 0px;
      width: 460px;
      height: 460px;
      overflow: auto;
      background-color: #ccc;
      padding: 20px;
    }
  }
</style>

```

### 如何在vue中引入jquery

#### 安装

> 在package.json里的dependencies加入"jquery" : "^2.2.3"，然后install 或者直接安装也可以

```
在webpack.base.conf.js里加入

var webpack = require("webpack")

在module.exports的最后加入

plugins: [
new webpack.ProvidePlugin({
jQuery: "jquery",
$: "jquery"
})
]

然后一定要重新 run dev

在main.js 引入就ok了import $ from 'jquery'

```

## 生命周期以及函数

### vue全局拦截前端错误（前端异常监控系统+ng4）

#### errorHandler

> 一般来讲，前端的异常处理使用的是try catch 和window.onerror 但是在框架中就不可以了，三大前端框架只有react可以这么使用，vue中有自己的errorHandler，ng也有自己的属性可以这么用

> 这里面我们用errorHandler来进行错误拦截

```
Vue.config.errorHandler = function (err, vm, info) {
  // handle error
  // `info` 是 Vue 特定的错误信息，比如错误所在的生命周期钩子
  // 只在 2.2.0+ 可用
}

```
```
//具体报错信息

ReferenceError: i is not defined  //err
    at VueComponent.created (bigdata.vue?1536:26)
    at callHook (vue.esm.js?5425:2895)
    at VueComponent.Vue._init (vue.esm.js?5425:4560)
    at new VueComponent (vue.esm.js?5425:4728)
    at createComponentInstanceForVnode (vue.esm.js?5425:4242)
    at init (vue.esm.js?5425:4059)
    at createComponent (vue.esm.js?5425:5512)
    at createElm (vue.esm.js?5425:5460)
    at createChildren (vue.esm.js?5425:5586)
    at createElm (vue.esm.js?5425:5488)

VueComponent {_uid: 4, _isVue: true, $options: {…},  //vm _renderProxy: Proxy, _self: VueComponent, …}


created hook  //info


```
[vue2.0参考这里](https://cn.vuejs.org/v2/api/#errorHandler )

[ng4参考这里](https://www.angular.cn/api/core/ErrorHandler)

#### 接口形式同埋点上传（利用一个gif的形式传递数据）

[这是我以前写的关于如何进行埋点上传的文章](https://hughendman.github.io/tags/%E5%9F%8B%E7%82%B9/)

### vue中操作DOM

#### 方案1

> 可以在mounted中挂载

```
mounted:function(){
        this.$nextTick(function(){         //this.$nextTick是在下次DOM更新循环结束时调用延迟回调函数。异步函数
            this.loadData();　　　　　　　　　　//DOM加载就绪，后调用loadData方法进行数据更新<br>//想要更新后的获取dom　//此时若获取更新后dom数据将会报错，数据为undefined；
        })
    }

```

#### 方案2

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


## error

### eslint语法解决

```
在webpack.base.conf.js里面删掉下面:

preLoaders: [
      {
        test: /\.vue$/,
        loader: 'eslint',
        include: projectRoot,
        exclude: [/node_modules/, /ignore_lib/]
      },
      {
        test: /\.js$/,
        loader: 'eslint',
        include: projectRoot,
        exclude: [/node_modules/, /ignore_lib/]
      }
    ]
```

```

删除以下代码就可以
{

    test: /\.(js|vue)$/,
    loader: 'eslint-loader',
    enforce: 'pre',
    include: [resolve('src'), resolve('test')],
    options: {
      formatter: require('eslint-friendly-formatter')
    }
```

> 然后需要重新编辑生效


## 杂项知识


### 大公司vue前端架构

vue + vuex+ axios + vue-router + webpack + es6 + less

### 单页面应用

> 单页 Web 应用 (single-page application 简称为 SPA) 是一种特殊的 Web 应用。它将所有的活动局限于一个Web页面中，仅在该Web页面初始化时加载相应的HTML、JavaScript 和 CSS。一旦页面加载完成了，SPA不会因为用户的操作而进行页面的重新加载或跳转。而是利用 JavaScript 动态的变换HTML的内（采用的是div切换显示和隐藏），从而实现UI与用户的交互。由于避免了页面的重新加载，SPA 可以提供较为流畅的用户体验。得益于ajax，我们可以实现无跳转刷新，又多亏了浏览器的histroy机制，我们用hash的变化从而可以实现推动界面变化。

### 前端路由的两种方式history 和 hash

[文章](https://www.cnblogs.com/photon-phalanx/p/7452331.html)
vue里面是有的是hash ，因为有一个#

> hash —— 即地址栏 URL 中的 # 符号（此 hash 不是密码学里的散列运算）。
比如这个 URL：http://www.abc.com/#/hello，hash 的值为 #/hello。它的特点在于：hash 虽然出现在 URL 中，但不会被包括在 HTTP 请求中，对后端完全没有影响，因此改变 hash 不会重新加载页面。

> history —— 利用了 HTML5 History Interface 中新增的 pushState() 和 replaceState() 方法。（需要特定浏览器支持）
这两个方法应用于浏览器的历史记录栈，在当前已有的 back、forward、go 的基础之上，它们提供了对历史记录进行修改的功能。只是当它们执行修改时，虽然改变了当前的 URL，但浏览器不会立即向后端发送请求。

### hash值

即url中#号后面的部分。

```
<a href="target">go target</a>

......
<div id="target">i am target place</div>
```
 
点击a链接，文档会滚动到id为target的div的可视区域上面去。hash除了这个功能还有另一一种含义：指导浏览器的行为但不上传到服务器。大家都知道，改变url中的任何一个字符都会导致浏览器重新请求服务器，除了#号后面那段字符之外。所以，简而言之我们可以这样理解：改变#后面的值不触发网页重载，但会记录到浏览器history中去。

> 驱动div显示隐藏的方式有很多种，比较好的选择为以下两种：

* 监听地址栏中hash变化驱动界面变化

* 用pushsate记录浏览器的历史，驱动界面发送变化


### 如何搭建一个基础的SPA
> Hash的改变不会引起界面的刷新，但是会出发onhashchange事件，我们要做的就是监听这个事件：

```
function hashChanged(){
    //变化之后的url
    var newhash = hashObj.newURL.split('#')[1];
    //变化之前的rul
    var oldhash = hashObj.oldURL.split('#')[1];
    //将对应的hash下界面显示和隐藏
    document.getElementById(oldhash).style.display = "none";
    document.getElementById(newhash).style.display = "block";
}
//监听路由变化

window.onhashchange = hashChanged;

```

### 改造vue项目为history模式(使用的是vue-cli)


修改 build/dev-server.js 文件

```
	
app.use(require('connect-history-api-fallback')())

```

这句话修改为

```
var history = require('connect-history-api-fallback')
app.use(history({
  rewrites: [
    { from: 'index', to: '/index.html'}, // 默认入口
    { from: /\/backend/, to: '/backend.html'}, // 其他入口
    { from: /^\/backend\/.*$/, to: '/backend.html'},
  ]
}))

```

[参考](https://github.com/bripkens/connect-history-api-fallback)

## 参考

[Vue2.0的三种常用传值方式、父传子、子传父、非父子组件传值](https://blog.csdn.net/lander_xiong/article/details/79018737)

[vuex参考文档](https://vuex.vuejs.org/zh/guide/index.html)