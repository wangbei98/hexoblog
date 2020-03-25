---
title: NLP_primer
author: bellick
copyright: true
top: 0
date: 2018-12-18 19:02:04
categories:
- MachineLearning
- NLP
tags:
- NLP
mathjax:
---
# NLP 入门
## 字符串操作
1. .count() 方法返回特定的子串在字符串中出现的次数。

```
seq = '12345,1234,123,12,1'
seq1 = '1'
a = seq.count(seq1)
a
>> 5
 
```
2. .strip()方法可以去除字符串首尾的指定符号。无指定时，默认去除空格符 ' ' 和换行符 '\n'。

```
seq = ' hello world!'
seq.strip()
>>helloworld!
seq.strip('!')
>>helloworld
seq.strip('hello')
>>world

```
3. 有时候只想要去除字符串开头的某个字符串，但是字符串的末尾有一个同样的字符串并不需要去掉。这时候可以使用 .lstrip() 方法。

```
seq = '12321'
seq.lstrip('1')
>>2321
```
4. 同样，可以使用.rstrip() 方法来单独去除末尾的字符

```
seq.rstrip('1')
>>1232
```
5. 经常会遇到需要将字符串拼接起来的情况，这时可以用运算符 + 来简单暴力的拼接。

```
seq1 = 'a'
seq2 = 'b'
seq3 = 'c'
seq = seq1 + seq2 + seq3
seq
>>abc
```
6. 需要将字符串用特定的符号拼接起来的字符的时候，可以用 .join() 方法来进行拼接

```
seq = ['2018', '10', '31']
seq = '-'.join(seq)  # 用 '-' 拼接
seq
>>2018-10-31
```
7. 当想要比较两个字符串的大小时，这里需要加载 operator 工具，它是 Python 的标准库。不需要额外下载，直接通过 import 调用即可。operator 从左到右第一个字符开始，根据设定的规则比较，返回布尔值（ True，False ）。

```
import operator
seq1 = '字符串1号'
seq2 = '字符串2号'
operator.lt(seq1, seq2)
operator.le(seq1, seq2)
operator.eq(seq1, seq2)
operator.ne(seq1, seq2)
operator.gt(seq1, seq2)
```
8. 除了使用 operator 之外，还有一种方法更简便: 直接使用运算符比较。
直接使用运算符比较 a < b:

```
seq1<seq2
seq1<=seq2
seq1==seq2
seq1!=seq2
seq1>seq2
```
9. 转化大小写

```
seq = 'appLE'
seq = seq.upper()
seq = seq.lower()
seq
```
10. 为了查找到某段字符串当中某个子串的位置信息，有两种方法。一种是.index ，一种是 .find。 两种方法都可实现这个功能，不同的是 index 如果未找到的话，会报错，而 find 未找到的则会返回 -1 值。

```
seq = '这个是一段字符串'
seq1 = '字符串'
seq.find(seq1)
seq3 = '字符串'
seq.index(seq3)
```
11. 截取字符串

```
seq = '这是字符串'
seq1 = seq[0:4]
seq1
seq2 = seq[0]
seq2

seq = '小了白了兔'
a = seq[0]
b = seq[2]
c = seq[4]
seq1 = a+b+c
seq1

seq = '今天天气很好，我们出去玩'
seq = seq.split('，')
seq
# 翻转
seq = '12345'
seq = seq[::-1]
seq
```

12.遇到需要判断某子串在字符串中是否出现，并根据判断做出后续操作的情况。可以用 in 来作出判断，若存在则返回 True，不存在则返回 False，然后配合 if ，作出后续操作

```
seq = 'abcdef'
'a' in seq
```
13. 有时需要把字符串中的某段字符串用另一段字符串代替，比如 2018-01-01 中的 - 用 '/' 代替。我们可以用到 .replace(a,b) ，他可以将某字符串中的 a 字符串 替换成 b 字符串。下面来实现一下。

```
seq = '2018-11-11'
seq = seq.replace('-', '/')
seq
```
14. 当遇到需要判断字符串是否以某段字符开头的时候。比如想要判断‘abcdefg’是否以 'a'开头。可以用 .startswish() 方法。

```
seq = 'abcdefg'
seq.startswith('a')
>>true
seq = 'abcd'
seq.endswith('d')
```
15. 有时候，当想要检查字符串的构成，像是检查字符串是否由纯数字构成

```
seq = 's123'
seq.isdigit()
```
## 正则表达式
# 加载 re 模块
1. 首先，我们需要通过 re.compile() 将编写好的正则表达式编译为一个实例。

```
import re

# 将正则表达式编写成实例
pattern = re.compile(r'[0-9]{4}')
pattern
```
2. 然后我们对字符串进行匹配，并对匹配结果进行操作。这里，我们要用到的是 re.search() 方法。 这个方法是将正则表达式与字符串进行匹配，如果找到第一个符合正则表达式的结果，就会返回，然后匹配结果存入group()中供后续操作。

```
import re
pattern = re.compile(r'[0-9]{4}')

time = '2018-01-01'

# 用刚刚编译好的 pattern，去匹配 time
match = pattern.search(time)

# 匹配结果存放在group()当中的
match.group()
>>2018
```
3. .findall()：

这个方法可以找到符合正则表达式的所有匹配结果。这里我们使用了 \d 规则的正则表达式，这个正则表达式可以替我们识别数字。

```
import re

pattern = re.compile(r'\d')

pattern.findall('o1n2m3k4')
>>['1', '2', '3', '4']

#同样的方法，我们编写一个 \D 正则表达式，这个可以匹配一个非数字字符。
pattern = re.compile('\D')

pattern.findall('1A2B3C4D')
>>['A', 'B', 'C', 'D']
```

4. .match() 方法与 .search() 方法类似，只匹配一次，并且只从字符串的*开头开始匹配*。同样，match 结果也是存在 group() 当中。

```
# 不止是规则，字符也是可以单独作为正则表达式使用。
pattern = re.compile('co')
pattern.match('comcdc').group()
>>'co'

pattern = re.compile('o')
pattern.match('comcdc').group()
>>error

```