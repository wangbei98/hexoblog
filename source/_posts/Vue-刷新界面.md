---
title: Vue-刷新界面
author: bellick
copyright: true
top: 0
date: 2020-05-19 21:15:27
categories:
- Vue
tags:
- Vue
mathjax:
---

# Vue-刷新界面

## 需求

在完成一些修改操作后，需要刷新界面以展现修改过后的界面

## 解决方案

```js
//App.vue中

<template>
  <div id="app">
    <router-view v-if="isRouterAlive"/>
  </div>
</template>

<script>
export default {
  name: 'App',
  provide () {
    return {
      reload: this.reload
    }
  },
  data () {
    return {
      isRouterAlive: true
    }
  },
  methods: {
    reload () {
      this.isRouterAlive = false
      this.$nextTick(function () {
        this.isRouterAlive = true
      })
    }
  }
}
</script>


```

```js
// 某页面

<script>

export default{
	name:'xxx'
	inject:['reload'],


	methods:{
		xxxx(){
			//
			// 需要需要刷新页面的地方调用 reload 函数
			//
			// 刷新页面
          	this.reload()
		}
	}
}

</script>

```