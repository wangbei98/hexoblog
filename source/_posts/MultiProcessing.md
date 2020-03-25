---
title: MultiProcessing
author: bellick
copyright: true
top: 0
date: 2019-05-03 20:08:39
categories:
- Notebook
- Python 
tags:
mathjax:
---
## Python 并行处理
#### 1. 使用concurrent.futures库
```
import concurrent.futures
# 创建Process Pool，默认为电脑的每个CPU创建一个
with concurrent.futures.ProcessPoolExecutor() as executor:
# 处理文件列表，但通过Process Pool划分工作，使用全部CPU！
eg.
film_ratings = {}
f_ids = film_ids
with concurrent.futures.ProcessPoolExecutor() as executor:
    for id,film_rating in zip(f_ids,executor.map(get_film_rating,f_ids)):
#         film_rating = get_film_rating(id)
        ratings = pd.DataFrame()
        ratings['cid'] = list(film_rating.keys())
        ratings['rate'] = list(film_rating.values())
        ratings.to_csv(id+'.csv',index=None,encoding='utf-8')
        film_ratings[id] = film_rating
        time.sleep(30)
    

```

## 2. MultiProcessing库