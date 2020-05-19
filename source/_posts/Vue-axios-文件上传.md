---
title: Vue-axios-文件上传
author: bellick
copyright: true
top: 0
date: 2020-05-19 21:03:27
categories:
- Vue
tags:
- Vue
mathjax:
---


# Vue-axios-文件上传

## 需求

实现上传文件

## 解决

将文件数据封装到表单数据中，再提交post

本例基于 axios 和 vuex，用户点击上传按钮触发 handleUpload 函数，handleUpload函数调用 store.js 中的函数 upload 进行上传

```js

// 上传按钮的 handler

handleUpload(){
	console.log('handle upload file is ')
	console.log(this.file)
	this.dialogVisible = false
	if(!this.file){
	  return
	}
	this.$store.dispatch('upload',{
	  file:this.file
	}).then(response => {
	  //成功后提示
	  // this.showuploadSuccessAlert()
	  this.$message({
	    message: '上传成功',
	    type: 'success'
	  })
	  // 刷新页面
	  this.reload()
	}).toLocaleString(err => {})
}
```

```js
//store.js

upload(context,data){
	let config = {
	headers: {
	  'Content-Type': 'multipart/form-data'
	}
	}
	console.log('action : upload courseware')
	console.log(data.file)
	let formData = new FormData();
	formData.append('file',data.file)
	console.log('action: submit upload')
	console.log(formData)
	return new Promise((resolve,reject) => {
	axios.post('/coursewares',formData,config)
	.then(response=>{
	  // context.commit('reNameFile',data)
	  console.log(response)
	  resolve(response)
	}).catch(err => {
	  console.log(err)
	  reject(err)
	})
})
```