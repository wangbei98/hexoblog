---
title: Vue-指令
author: bellick
copyright: true
top: 0
date: 2020-04-11 14:24:22
categories:
- Vue
tags:
- Vue
mathjax:
---



# Vue-指令



在html中，常常为html标签添加属性/参数，比如 a标签，用href 设置url

在vue 中，为了可以动态地设置这些参数，可以使用 v- xxx 



```html
<div id="app">
	<p v-if="seen">现在你看到我了</p>
	<a v-bind:href="url"> ... </a>
	<div @click="click1">
		<div @click.stop="click2">
			clickme
		</div>
	</div>
</div>
<script>
var app = new Vue({
	el:'#app',
	data:{
		seen:false,
		url:'baidu.com'
	},
	methods:{
		click1:function(){
			console.log('click1...')
		},
		click2:function(){
			console.log('click2...')
		}
	}
})
</script>
```



## 缩写

### [`v-bind` 缩写](https://cn.vuejs.org/v2/guide/syntax.html#v-bind-缩写)

```
<!-- 完整语法 -->
<a v-bind:href="url">...</a>

<!-- 缩写 -->
<a :href="url">...</a>

<!-- 动态参数的缩写 (2.6.0+) -->
<a :[key]="url"> ... </a>
```

### [`v-on` 缩写](https://cn.vuejs.org/v2/guide/syntax.html#v-on-缩写)

```
<!-- 完整语法 -->
<a v-on:click="doSomething">...</a>

<!-- 缩写 -->
<a @click="doSomething">...</a>

<!-- 动态参数的缩写 (2.6.0+) -->
<a @[event]="doSomething"> ... </a>
```