---
title: Git内部原理
tags:
	- Git
---

Git本质上是一个内容寻址（content-addressable）文件系统，并在此上提供了一个版本控制系统的用户界面。

## 底层命令与高级命令

`.git`目录包含了几乎所有Git存储和操作的东西。如果向备份或复制一个版本库，只需把这个目录拷贝至另一处即可。`.git`目录结构如下：

```bash
config
description
HEAD
hooks/
info/
objects/
refs/
```

`description`文件仅供GitWeb程序使用。`config`文件描述项目特有的配置选项。`info`保存了一个全局排除（global exclude）文件和refs记录本地HEAD以及remote的HEAD。 `hooks`包含脚本。

`HEAD`指向目前被检出的分支。`index`文件保存暂存信息。`objects`存储所有数据内容。`refs`存储指向数据（分支、远程仓库和标签等）的提交对象的指针。

## Git对象

Git的核心是一个键值对数据库（key-value data store）。可以向Git仓库中插入任意类型的内容，它会返回一个唯一的键，通过该键可以在任意时刻再次取回该内容。

### 数据对象（blob object）

```bash
$ echo "version 1" | git hash-object -w --stdin
```

```bash
$ git cat-file -p
```



### 树对象（tree object）



### 提交对象 （commit Object）




## Git可视化网站，帮助理解

[Visualizing Git Concepts with D3](http://onlywei.github.io/explain-git-with-d3/#rebase)

[图解Git](https://marklodato.github.io/visual-git-guide/index-zh-cn.html?no-svg)

## Reference

[Pro Git](https://git-scm.com/book/en/v2/Git-Internals-Plumbing-and-Porcelain)