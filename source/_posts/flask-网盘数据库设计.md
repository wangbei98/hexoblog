---
title: flask-网盘数据库设计
author: bellick
copyright: true
top: 0
date: 2020-03-26 10:35:18
categories:
- flask
- flask-disk
tags:
- flask
mathjax:
---

# flask-网盘数据库设计

## 数据库设计

使用的是 MongoDB 文档型数据库(Mysql属于关系型数据库)。由于本次实现的是一个云盘，主要是目录与子目录的关系，所以整体是一个树状结构。
在设计数据库时，并不是通过维护一个嵌套的对象，而是将所有的数据都放入到一维数组，父子关系通过`parId`字段来维护，根目录到当前所在目录通过`pathRoot`字段来维护。只考虑相邻的两级关系，不考虑所有的层级关系，优势如下：

1. 存入数据时，只需要将`parId`设为当前目录的Id，`pathRoot`设为根目录到当前目录的路径。而不是通过递归对象将数据放到对应的节点上。
2. 修改当前文件或者目录名称时，只需要根据当前数据的Id去数据库匹配然后修改。如果修改的是目录名称(假如nameA修改为nameB)，会影响该目录下所有子目录和子文件的`pathRoot`字段，那么只需要匹配所有满足`pathRoot`字段里面包含nameA的数据，然后替换成nameB就可以了。
3. 移动文件或目录时，所有的子目录或者文件也会跟着变化。修改当前文件或目录的`parId`字段，指向新的目录Id，同样也要修改所有子目录或者子文件的`pathRoot`字段，同上。
4. 删除文件或目录时，根据当前的Id去数据库匹配删除。如果删除的目录里面有子目录或者文件(假如删除的是目录folderA)，那么只需要匹配所有满足`pathRoot`字段里面包含folderA的数据，然后全部删除。最后记得也要删除磁盘文件，不然内存消耗太大(如果有回收站保留30天，则设置最后保留时间，超时后再去删磁盘文件)。



| FileNode表 |                                            |                |
| ---------- | ------------------------------------------ | -------------- |
| id         | 编号                                       | primaryKey     |
| name       |                                            |                |
| path_root  | 根目录到当前目录到路径（不包含当前文件名） |                |
| parent_id  | 父目录的id                                 | foreign key    |
| is_folder  | 是否是文件夹                               | default = true |
|            |                                            |                |
| User_id    | 所属用户ID                                 |                |

```
/
|---root
	|--- folder
			|--- test.py
```

* 特别地，对于根目录 / 
  * ID = 0, 	name = 'root' , 	path_root='/',	parent_id=-1,	is_folder = True
  * 根目录完整路径 path_root + name = '/root'
* 文件夹 folder
  * id = 0, name='folder1', path_root='/root/' , is_folder = True
  * 完整路径 path_root + name = '/root/folder'
* 文件 Test.py
  * id = 1, name = 'text.py', path_root = '/root/folder/', is_folder = false
  * 完整路径 path_root + name = '/root/folder/text.py'