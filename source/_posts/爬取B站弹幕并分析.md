---
title: 爬取B站弹幕并分析
author: bellick
copyright: true
top: 0
date: 2020-12-23 17:09:30
categories:
tags:
mathjax:
---

# 爬取B站弹幕

## 0.需求

1. 爬取B站上4个视频从上传开始7天内的弹幕
2. 分析弹幕的极性
3. 分词并做词云展示

## 1. 爬虫

### 分析

Bilibili视频中的弹幕是和视频内容分开的，有专门的接口

> https://api.bilibili.com/x/v2/dm/history?type=1&oid={cid}&date={date}

通过cid(应该和oid是一个东西)和日期就可以获取该视频在某一天的弹幕（但貌似最多1000天 

### 所需的包

```python
import requests
import parsel
import csv
import jieba.analyse
import wordcloud
from matplotlib import pylab as plt
import time,datetime
import re
from datetime import *
import pandas as pd
import re
from bs4 import BeautifulSoup
from snownlp import SnowNLP
import numpy as np
from PIL import Image
import pyecharts.options as opts
from pyecharts.charts import WordCloud
from pyecharts.globals import SymbolType
from collections import Counter
```

### 4个视频的BV号

```python
bids = ['BV1A7411e71Y','BV1H7411Y7kG','BV1o741167d7','BV1S7411V7MD']
```

### 通过BV号获取cid

```
def get_cid(bvid):
    '''
    通过视频的bvid获得视频的cid
    输入：视频的bvid
    输出：视频的cid
    '''
    url = 'https://api.bilibili.com/x/player/pagelist?bvid=%s&jsonp=jsonp'%bvid
    res = requests.get(url)
    data = res.json()
    return data['data'][0]['cid']
```

### 获取视频信息

```python

'''
通过视频的bvid获得视频的 播放量、弹幕量、上传时间
'''
def get_info(bvid):
    url = 'https://www.bilibili.com/video/'+bvid
    headers = {"User-Agent": "Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/83.0.4103.116 Safari/537.36",
         "cookie": "登陆的cookie"}
    
    
    response=requests.get(url,headers=headers)
    html=response.content.decode("utf-8")
    soup=BeautifulSoup(html)
    video_data = soup.find_all("div", class_="video-data")[0]
    spans = video_data.find_all("span")
    play_num = spans[0].string
    dm_num = spans[1].string
    upload_time = spans[2].string
#     spans = re.findall('<span title="(.*?)" class="(.*?)">(.*?)</span>',text)
    return play_num,dm_num,upload_time
```

### 解析弹幕文件并进行极性分析

```python
def parse_dm(text):
    '''
    解析视频弹幕
    输入：视频弹幕的原数据
    输出：弹幕的解析结果
    '''
    result = [] # 用于存储解析结果
    data = re.findall('<d p="(.*?)">(.*?)</d>',text)
    for d in data:
        item = {} # 每条弹幕数据
        dm = d[0].split(',') # 弹幕的相关详细，如出现时间，用户等
        item['出现时间'] = float(dm[0])
        item['模式'] = int(dm[1])
        item['字号'] = int(dm[2])
        item['颜色'] = int(dm[3])
        item['弹幕池'] = int(dm[5])
        item['用户ID'] = dm[6] # 并非真实用户ID，而是CRC32检验后的十六进制结果
        item['rowID'] = dm[7] # 弹幕在数据库中的ID，用于“历史弹幕”功能
        item['弹幕内容'] = d[1]
        item['sentiments'] = SnowNLP(d[1]).sentiments # 对弹幕进行极性分析
        result.append(item)    
    return result
```

### 获取指定日期的弹幕

```python
def get_history(bvid,date): 
    '''
    获取视频历史弹幕
    输入：视频bvid,日期
    输出：视频某一日期开始的弹幕
    '''
    oid = get_cid(bvid)
    url = 'https://api.bilibili.com/x/v2/dm/history?type=1&oid=%d&date=%s'%(oid,date)
    headers = {"User-Agent": "Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/83.0.4103.116 Safari/537.36",
         "cookie": "LIVE_BUVID=AUTO3815462686837394; rpdid=ilpmiiwpoodospxixqmxw; dsess=BAh7CkkiD3Nlc3Npb25faWQGOgZFVEkiFTM3ODBhNzljYzZhMWJhZTAGOwBG%0ASSIJY3NyZgY7AEZJIiVhNWEzYjE0N2ZmMDRkNDgwMGRkZGJiNjA0OGQ3ZjE0%0AZAY7AEZJIg10cmFja2luZwY7AEZ7B0kiFEhUVFBfVVNFUl9BR0VOVAY7AFRJ%0AIi00N2JhMDNiODNlNTE2ZjhiMjA4NDkyNWM4ZjhmZTk2MzVmZjhlZGY3BjsA%0ARkkiGUhUVFBfQUNDRVBUX0xBTkdVQUdFBjsAVEkiLTdlNDI3MWM1NWZkYmQ2%0AYTE5ZjFjZGY2MDA4OTRlYmFmODA1NzYwZWIGOwBGSSIKY3RpbWUGOwBGbCsH%0ATsYuXUkiCGNpcAY7AEYiFDIxOS4yMTcuMTQ3LjEwMQ%3D%3D%0A--449de1a1940df57d3e8b9211ec56858bd928d7af; CURRENT_QUALITY=80; blackside_state=1; CURRENT_FNVAL=80; _uuid=34EFDC48-F4BD-D35E-3235-C8AE0DB4CCA905036infoc; buvid3=510B5C99-E9FB-49EB-8D00-414C80E44C58143106infoc; DedeUserID=363346252; DedeUserID__ckMd5=1d70b6816b50a8c6; SESSDATA=7ecdc960%2C1620726622%2C544c6*b1; bili_jct=ec1c1e5865981a737f39ba217f95656f; bp_t_offset_363346252=468635259037936097; PVID=1; sid=i4xybs7q; bsource=search_google"}
    
    # 此接口需要登陆，因此需要cookies
    cookies = {}
    res = requests.get(url,headers=headers)
    res.encoding = 'utf-8'
    text = res.text
    dms = parse_dm(text) # 解析弹幕
    return dms
```

### 爬取某视频的弹幕并存储

```python
def save_dm(bvid):
    print('视频bvid = %s'%bvid)
    print('视频解析中...')
    info = get_info(bvid)
    print('视频解析完成!')
    play_num,dm_num,upload_time = get_info(bvid)
    print('【视频播放量】：%s\n【弹幕数量】：  %s\n【上传日期】：  %s'%(play_num,dm_num,upload_time))
    upload_date = datetime.strptime(upload_time,"%Y-%m-%d %X")
    dms = []
    # 从视频上传的那天开始，爬取7天的弹幕
    for day in range(7):
        # 遍历的那天
        cur_date = upload_date + timedelta(days=day)
        # 日期转为字符串
        cur_date_str = datetime.strftime(cur_date,'%Y-%m-%d')
        cur_dm = get_history(bvid,cur_date_str)
        dms.extend(cur_dm)
    print('弹幕爬取完成!(共%d条)'%len(dms))
    print('存储到csv...')
    my_df = pd.DataFrame(dms)
    my_df.to_csv(bvid+'.csv',index=True,sep=',')
    print('存储完成...')
```

### 执行爬虫

```python
for bid in bids:
    save_dm(bid)
```

> ```
> 视频bvid = BV1A7411e71Y
> 视频解析中...
> 视频解析完成!
> 【视频播放量】：1003.2万播放 · 
> 【弹幕数量】：  9.7万弹幕
> 【上传日期】：  2020-01-22 08:00:01
> 弹幕爬取完成!(共7000条)
> 存储到csv...
> 存储完成...
> 视频bvid = BV1H7411Y7kG
> 视频解析中...
> 视频解析完成!
> 【视频播放量】：886.6万播放 · 
> 【弹幕数量】：  18.1万弹幕
> 【上传日期】：  2020-01-24 17:30:52
> 弹幕爬取完成!(共10500条)
> 存储到csv...
> 存储完成...
> 视频bvid = BV1o741167d7
> 视频解析中...
> 视频解析完成!
> 【视频播放量】：429.3万播放 · 
> 【弹幕数量】：  7.4万弹幕
> 【上传日期】：  2020-01-27 22:55:11
> 弹幕爬取完成!(共6635条)
> 存储到csv...
> 存储完成...
> 视频bvid = BV1S7411V7MD
> 视频解析中...
> 视频解析完成!
> 【视频播放量】：164.8万播放 · 
> 【弹幕数量】：  2.9万弹幕
> 【上传日期】：  2020-02-13 09:00:29
> 弹幕爬取完成!(共10500条)
> 存储到csv...
> 存储完成...
> ```

## 2. 词云分析

### 加载停用词

```python
stopwords = pd.read_csv('stopwords.txt', encoding='gb2312', names=['stopword'], index_col=False)

stop_list = stopwords['stopword'].tolist()
```

### 分词

```python
'''
根据bvid从获取的弹幕文件中提取所有的弹幕，并分词
'''
def get_text(bvid):
#     tokenizer = MiNLPTokenizer(granularity='fine')
    csv_data = pd.read_csv(bvid+'.csv')
    dm_contents = csv_data['弹幕内容']
    text = ''
    for content in dm_contents:
        seg_list = jieba.cut(content)
#         seg_list = tokenizer.cut(content)
        text_list = " ".join(seg_list)
        text += " "
        text += text_list
    wordlist = text.split(" ")
    wordlist = [i for i in wordlist if i not in stop_list]
    text = " ".join(wordlist)
    with open(bvid+'.txt','w') as f:
        print('打开文件:'+bvid+'.txt')
        print('开始写入...')
        f.write(text)
        print('写入完成...')
```

### 执行分词

```python
for bid in bids:
    get_text(bid)
```

> ```
> 打开文件:BV1A7411e71Y.txt
> 开始写入...
> 写入完成...
> 打开文件:BV1H7411Y7kG.txt
> 开始写入...
> 写入完成...
> 打开文件:BV1o741167d7.txt
> 开始写入...
> 写入完成...
> 打开文件:BV1S7411V7MD.txt
> 开始写入...
> 写入完成...
> ```

### 制作词云图

```python
def word_cloud(bvid):
    #制作词云图
    f = open(bvid+'.txt','r')
    text = f.read()
    words = text.split(" ")
    # print(words)
    wd = Counter(words)
    wd_tuples = zip(wd.keys(),wd.values())
    c = (
        WordCloud()
        .add("", wd_tuples, word_size_range=[20, 100], shape=SymbolType.DIAMOND)
        .set_global_opts(title_opts=opts.TitleOpts(title="词频统计："+bvid))
        .render(bvid+".html")
    )
for bid in bids:
    word_cloud(bid)
```



### 统计弹幕极性

```python
'''
统计弹幕极性
'''
def tongji_senti(bvid):
    csv_data = pd.read_csv(bvid+'.csv')
    pos = 0
    neg = 0
    dm_sentis = csv_data['sentiments']
    for senti in dm_sentis:
        if senti >= 0.5:
            pos = pos+1
        else:
            neg = neg+1
    return pos,neg

poss = 0
negs = 0
for bid in bids:
    pos,neg = tongji_senti(bid)
    print('********************************')
    print(bid + ' 中共有弹幕' + str(pos+neg) +'条')
    print(bid + ' 中正面弹幕有' + str(pos) +'条')
    print(bid + ' 中负面弹幕有' + str(neg) +'条')
    poss += pos
    negs += neg
print('################################')
print('所有视频中正面弹幕有' + str(poss) +'条')
print('所有视频中负面弹幕有' + str(negs) +'条')
```

> ```
> ********************************
> BV1A7411e71Y 中共有弹幕7000条
> BV1A7411e71Y 中正面弹幕有5849条
> BV1A7411e71Y 中负面弹幕有1151条
> ********************************
> BV1H7411Y7kG 中共有弹幕10500条
> BV1H7411Y7kG 中正面弹幕有8981条
> BV1H7411Y7kG 中负面弹幕有1519条
> ********************************
> BV1o741167d7 中共有弹幕6635条
> BV1o741167d7 中正面弹幕有5337条
> BV1o741167d7 中负面弹幕有1298条
> ********************************
> BV1S7411V7MD 中共有弹幕10500条
> BV1S7411V7MD 中正面弹幕有8326条
> BV1S7411V7MD 中负面弹幕有2174条
> ################################
> 所有视频中正面弹幕有28493条
> 所有视频中负面弹幕有6142条
> ```



## 结果

![截屏2020-12-23 下午5.24.02](https://tva1.sinaimg.cn/large/0081Kckwgy1glxwz4kasaj30ng0d50zp.jpg)