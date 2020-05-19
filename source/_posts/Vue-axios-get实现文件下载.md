---
title: Vue-axios-get实现文件下载
author: bellick
copyright: true
top: 0
date: 2020-05-01 11:42:38
categories:
- Vue
tags:
- Vue
mathjax:
---

# Vue-axios-get实现文件下载

## 需求

后端实现了文件下载API： http://xxx.xxx.xxxx.xxxx/api/file/download?id=xx
前端携带用来认证的cookie，调用此GET请求，可以实现文件的下载（附件形式）

在vue中，使用a 标签可以发送get请求，但是不能携带cookie 数据，无法实现鉴权，因此需使用axios发送请求

## 实现

1. 请求下载的时候设置responseType为blob

```js
	// store/store.js

	downloadFile(context,id){
      return new Promise((resolve,reject) => {
        console.log('action: downloadFile')
        axios.get('/file/download?id='+id,{
          responseType: 'blob'
        })
        .then(response =>{
          resolve(response)
        }).catch(err => {
          console.log(err)
          reject(err)
        })
      })
    }
````

2. 模拟a标签点击，成功下载文件

```js
	//组件内 -- 事件处理函数 （methods）

	handleDownload(){
        console.log('in handleDownload')
        this.selected.forEach(id => this.$store.dispatch('downloadFile',id)
        .then(res => {
          console.log('in handleDownload -> res:')
          console.log(res)
          if (res.data.type === "application/json") {
            this.$message({
              type: "error",
              message: "下载失败，文件不存在或权限不足"
            });
          } else {
            if (window.navigator.msSaveOrOpenBlob) {
              navigator.msSaveBlob(blob, file.fileName);
            } else {
              let url = window.URL.createObjectURL(new Blob([res.data]))
              let link = document.createElement('a')
              link.style.display = 'none'
              link.href = url
              link.setAttribute('download', file.filename)

              document.body.appendChild(link)
              link.click()
            }
          }
          this.$message({
            message: '下载成功',
            type: 'success'
          })
        })
        .catch(err => {
          console.log(err)
          this.$message.error('下载失败，请刷新后重试');
        }))

      },
````


## 参考

https://juejin.im/post/5bc6e25d5188255c5a0d2adf
https://zhuanlan.zhihu.com/p/42204023