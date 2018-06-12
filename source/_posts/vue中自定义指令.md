---
title: vue中自定义指令
date: 2018-06-11 11:15:00
categories : VUE
tags: VUE
keywords : VUE
---

```
// 注册一个全局自定义指令 `v-focus`
Vue.directive('focus', {
  // 当被绑定的元素插入到 DOM 中时……
  inserted: function (el) {
    // 聚焦元素
    el.focus()
  }
})

```

```
directives: {
  focus: {
    // 指令的定义
    inserted: function (el) {
      el.focus()
    }
  }
}

```

```
<input v-focus>

```


这里举一个高深的例子

```
<div id="hook-arguments-example" v-demo:foo.a.b="message"></div>

```

```
Vue.directive('demo', {
  bind: function (el, binding, vnode) {
    var s = JSON.stringify
    el.innerHTML =
      'name: '       + s(binding.name) + '<br>' +
      'value: '      + s(binding.value) + '<br>' +
      'expression: ' + s(binding.expression) + '<br>' +
      'argument: '   + s(binding.arg) + '<br>' +
      'modifiers: '  + s(binding.modifiers) + '<br>' +
      'vnode keys: ' + Object.keys(vnode).join(', ')
  }
})

new Vue({
  el: '#hook-arguments-example',
  data: {
    message: 'hello!'
  }
})

```

简写模式

```

Vue.directive('color-swatch', function (el, binding) {
  el.style.backgroundColor = binding.value
})


```

接收数据

```
<div v-demo="{ color: 'white', text: 'hello!' }"></div>

```

```
Vue.directive('demo', function (el, binding) {
  console.log(binding.value.color) // => "white"
  console.log(binding.value.text)  // => "hello!"
})

```