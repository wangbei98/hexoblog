---
title: python_ProblemOfChineseReading
author: bellick
copyright: true
top: 0
date: 2019-03-26 21:23:46
categories:
tags:
mathjax:
---
# python 读取中文
## 策略1
```
with codecs.open('data/1.txt', 'r', 'GBK') as f:
    data = f.read()
    print(data)
```
## 策略2
```
import sys
reload(sys)
sys.setdefaultencoding("utf-8")
fin = open('data/.txt', 'r')
for eachLine in fin:
    line = eachLine.strip().decode('gbk', 'utf-8')
    print(line)

```
## 策略3
```
data = open(r'example.txt','r',encoding='utf-8').read()

# 文本预处理
pattern = re.compile(u'\t|\n|\.|-|:|;|\(|\)|\?|"')
data_text = re.sub(pattern,'',data_text)
```