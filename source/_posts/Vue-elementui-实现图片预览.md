---
title: Vue-elementui-实现图片预览
author: bellick
copyright: true
top: 0
date: 2020-05-01 14:10:07
categories:
- Vue
tags:
- Vue
- ElementUI
mathjax:
---

# Vue-elementui-实现图片预览

## 需求

展示图片缩略图，并且点击可以预览大图

## 实现 

使用 elementUI 的Image组件

```html
<div v-else-if="type=='img'">
    <el-image
      style="width: 50px; height: 50px"
      :src="previewURL"
      :preview-src-list="[previewSRC]"
      fit="contain"></el-image>
</div>

src 用来指定缩略题的url
preview-src-list 用来指定大图的url
```

```js
computed:{
	  previewURL(){
        return 'http://xxxx/api/file/preview?id=' + this.id +  '&width=50&height=50&token=' + this.$store.getters.token
      },
      //预览图
      previewSRC(){
        return 'http://xxxx/api/file/preview?id=' + this.id +  '&width=800&height=800&token=' + this.$store.getters.token
      }
}

```

## 结果

![](https://tva1.sinaimg.cn/large/007S8ZIlly1gecxam8jjfj30yl0hb7gl.jpg)