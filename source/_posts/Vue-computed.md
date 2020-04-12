---
title: Vue-computed
author: bellick
copyright: true
top: 0
date: 2020-04-11 14:56:43
categories:
- Vue
tags:
- Vue
mathjax:
---



# Vue-computed计算属性



## 理解

有时候需要对data中的数据做一些操作（或者有一些数据需要随着其它数据变动而变动时），为了方便起见，会把这些操作封装为计算属性

本质上是个属性

```html
<div id="app-13">
	<p>{{message}}</p>
	<p>{{reverseMessage}}</p>
</div>

<script>
var app13 = new Vue({
	el:'#app-13',
	data:{
		message:'hello'
	},
	computed:{
		reverseMessage:function(){
			return this.message.split('').reverse().join('')
		}
	}
})
</script>
```

当然这种操作也可以通过方法来完成: 在methods中新建方法

```html
<p>Reversed message: "{{ reversedMessage() }}"</p>
<script>
methods: {
  reversedMessage: function () {
    return this.message.split('').reverse().join('')
  }
}
</script>
```



https://cn.vuejs.org/v2/guide/computed.html

我们可以将同一函数定义为一个方法而不是一个计算属性。两种方式的最终结果确实是完全相同的。然而，不同的是**计算属性是基于它们的响应式依赖进行缓存的**。只在相关响应式依赖发生改变时它们才会重新求值。这就意味着只要 `message` 还没有发生改变，多次访问 `reversedMessage` 计算属性会立即返回之前的计算结果，而不必再次执行函数。

这也同样意味着下面的计算属性将不再更新，因为 `Date.now()` 不是响应式依赖：

```
computed: {
  now: function () {
    return Date.now()
  }
}
```

相比之下，每当触发重新渲染时，调用方法将**总会**再次执行函数。