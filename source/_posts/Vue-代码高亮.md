---
title: Vue-代码高亮
author: bellick
copyright: true
top: 0
date: 2020-05-19 20:55:40
categories:
- Vue
tags:
- Vue
mathjax:
---


# Vue-代码高亮

## 需求

在vue前端项目中，需要代码在界面中高亮显示.


## 解决方案

使用[highlight.js](https://highlightjs.org/)

1. 安装

```
npm install highlight.js --save
```

2. 封装成Vue插件

```

// src/utils/highlight.js 文件路径，纯属自定义

// highlight.js  代码高亮指令
import Hljs from 'highlight.js';
import 'highlight.js/styles/tomorrow-night.css'; // 代码高亮风格，选择更多风格需导入 node_modules/hightlight.js/styles/ 目录下其它css文件

let Highlight = {};
// 自定义插件
Highlight.install = function (Vue) {
    // 自定义指令 v-highlight
    Vue.directive('highlight', {
        // 被绑定元素插入父节点时调用
        inserted: function(el) {
            let blocks = el.querySelectorAll('pre code');
            for (let i = 0; i < blocks.length; i++) {
                Hljs.highlightBlock(blocks[i]);
            }
        },
        // 指令所在组件的 VNode 及其子 VNode 全部更新后调用
        componentUpdated: function(el) {
            let blocks = el.querySelectorAll('pre code');
            for (let i = 0; i < blocks.length; i++) {
                Hljs.highlightBlock(blocks[i]);
            }
        }
    })
};

export default Highlight;

```

3. 全局引入自定义插件

```
// highlight.js代码高亮插件

//在src/main.js中：

import Highlight from './utils/highlight'; // from 路径是highlight.js的路径，纯属自定义
Vue.use(Highlight);
```

4. 使用

```

<div id="codeView" v-highlight>
    <pre><code v-html="code"></code></pre>
</div>

```

## 参考

[链接](https://99mycql.github.io/application/%E9%80%9A%E8%BF%87highlight%E5%9C%A8vue%E4%B8%AD%E5%AE%9E%E7%8E%B0%E4%BB%A3%E7%A0%81%E9%AB%98%E4%BA%AE.html)