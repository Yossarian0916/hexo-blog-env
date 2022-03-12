---
title: "AOSP 入门"
date: 2022-03-12 12:14
tags:
    - AOSP
---

# AOSP 入门

### 源码下载

#### 国内同步UTSC源

第一次同步数据量特别大，如果网络不稳定，中间失败就要从头再来了。所以我们提供了打包的 AOSP 镜像，为一个 tar 包，大约 16G（单文件 16G，注意你的磁盘格式要支持）。这样你 就可以通过 HTTP 的方式下载，该方法支持断点续传。

下载地址 http://mirrors.ustc.edu.cn/aosp-monthly/

请注意对比 checksum。

然后根据下文 已有仓库如何改用科大源 的方法更改同步地址。

解压后用命令 repo sync 就可以把代码都 checkout 出来。

Note: tar 包为定时从 https://mirrors.tuna.tsinghua.edu.cn/aosp-monthly/ 下载。

#### 科学同步 googlesource

按照 Google 官方教程 https://source.android.com/source/downloading.html

`https://android.googlesource.com/platform/manifest` 

具体做法摘录如下（以防墙抽风）：

首先下载 repo 工具。

```bash
mkdir ~/bin
PATH=~/bin:$PATH
curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
```

然后建立一个工作目录（名字任意）

```bash
mkdir WORKING_DIRECTORY
cd WORKING_DIRECTORY
```

初始化仓库：

```bash
repo init
```

同步源码树（以后只需执行这条命令来同步）：

```bash
repo sync
```



### AOSP源码导入Android Studio

AOSP源码顶级目录下运行以下命令，生成google推荐的Idegen工具的配置文件`android.ipr`和`android.iml`

```bash
source build/envsetup.sh # 在新终端下需要执行这一步
mmma development/tools/idegen
development/tools/idegen/idegen.sh
```

**过滤一些模块**
 如果把Android所有的源码全部导入到IDE里面去，工程将会非常大，而且会很耗时间，那么我们就可以把不需要的模块给过滤掉。
 打开`android.iml`文件，加入以下代码，修改`excludeFolder`的配置：

```bash
<excludeFolder url="file://$MODULE_DIR$/.repo"/>
<excludeFolder url="file://$MODULE_DIR$/abi"/>
<excludeFolder url="file://$MODULE_DIR$/frameworks/base/docs"/>
<excludeFolder url="file://$MODULE_DIR$/art"/>
<excludeFolder url="file://$MODULE_DIR$/bionic"/>
<excludeFolder url="file://$MODULE_DIR$/bootable"/>
<excludeFolder url="file://$MODULE_DIR$/build"/>
<excludeFolder url="file://$MODULE_DIR$/cts"/>
<excludeFolder url="file://$MODULE_DIR$/dalvik"/>
<excludeFolder url="file://$MODULE_DIR$/developers"/>
<excludeFolder url="file://$MODULE_DIR$/development"/>
<excludeFolder url="file://$MODULE_DIR$/device"/>
<excludeFolder url="file://$MODULE_DIR$/docs"/>
<excludeFolder url="file://$MODULE_DIR$/external"/>
<excludeFolder url="file://$MODULE_DIR$/hardware"/>
<excludeFolder url="file://$MODULE_DIR$/kernel-3.18"/>
<excludeFolder url="file://$MODULE_DIR$/libcore"/>
<excludeFolder url="file://$MODULE_DIR$/libnativehelper"/>
<excludeFolder url="file://$MODULE_DIR$/ndk"/>
<excludeFolder url="file://$MODULE_DIR$/out"/>
<excludeFolder url="file://$MODULE_DIR$/pdk"/>
<excludeFolder url="file://$MODULE_DIR$/platform_testing"/>
<excludeFolder url="file://$MODULE_DIR$/prebuilts"/>
<excludeFolder url="file://$MODULE_DIR$/rc_projects"/>
<excludeFolder url="file://$MODULE_DIR$/sdk"/>
<excludeFolder url="file://$MODULE_DIR$/system"/>
<excludeFolder url="file://$MODULE_DIR$/tools"/>
<excludeFolder url="file://$MODULE_DIR$/trusty"/>
<excludeFolder url="file://$MODULE_DIR$/vendor"/>
```

这样我们就只导入了`frameworks`和`packages`的代码。

AOSP工程项目很大，在导入源码到IDEA之前需要先修改IDEA的配置：
 修改内存大小：
 打开IDEA 菜单栏`Help` >`Edit Custom VM Options`，添加

```bash
-Xms1g 
-Xmx5g
```

修改文件大小限制，打开区分大小写选项:
 打开IDEA 菜单栏 `Help -> Edit custom properties`， 添加

```bash
idea.max.intellisense.filesize=100000
idea.case.sensitive.fs=true
```

> NOTE: 重启IDEA使配置生效

用IDEA找到AOSP目录下的`development/tools/idegen/android.ipr`文件，打开AOSP工程，耐心等待，索引需要一定时间




## Reference
[AOSP(Android) 镜像使用帮助](https://lug.ustc.edu.cn/wiki/mirrors/help/aosp/)
[AOSP Intellij IDE 导入源码阅读](https://www.jianshu.com/p/3e322f20da5b)
