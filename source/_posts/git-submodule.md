---
title: Git Submodule
date: 2020-10-10 12:54:36
tags:
  - Git

---


# Git Submodule

有可能会将代码根据功能拆解成不同的子模块。主项目对子模块有依赖关系，却又并不关心子模块的内部开发流程细节。

这种情况下，通常不会把所有源码都放在同一个 Git 仓库中。

有一种比较简单的方式，是在当前工作目录下，将子模块文件夹加入到 `.gitignore` 文件内容中，这样主项目就能够无视子项目的存在。这样做有一个弊端就是，使用主项目的人需要有一个先验知识：需要在当前目录下放置一份某版本的子模块代码。

还有另外一种方式可供借鉴，可以使用 Git 的 `submodule` 功能

### 1. 创建submodule

`git submodule add <submodule_url> <directory>`在项目中创建一个子模块

例如：在project目录下`git submodule add github.com/username/projectA moduleA`

此时主项目仓库会多出两个文件：`.gitmodules`和`moduleA`, `.gitmodules`包含submodule的主要信息

```markdown
[submodule "ModuleA"]
path=moduleA
url=github.com/username/projectA
```

`moduleA`文件记录子模块的commit id。父项目并不会记录submodule的文件变动，只是按照commit id指定子模块的git HEAD。所以`.gitmodules`和`moduleA`都是需要提交到父项目的远程仓库中去的

```bash
git add .gitmodules moduleA
git commit -m 'add submodule projectA'
```

### 2. 克隆带子模块的repo

方法一：先clone父项目，再初始化submodule，最后更新submodule。初始化只做一次，之后每次只需要update。默认submodule不在任何分支上，它指向父项目保存的submodule commit id

```bash
git clone github.com/username/project && cd project
git submodule init
git submodule update
cd ..
```

方法二：带上参数`--recursive-submodules`

`git clone github.com/username/project --recursive-submodules`

### 3. 修改submodule

修改子模块代码只对子模块的版本库产生影响，对父项目的版本库不会产生任何影响。在子项目中修改代码并提交，之后在父项目中便可以看到子项目的新提交，正常add，commit就好。如果父项目需要用到最新版本的子模块，我们需要更新`.gitmodules`中submodule commit id。

### 4. 更新submodule

子模块的默认分支不是master

方法一：先pull父项目，然后执行`git submodule update`，注意moduleA的分支

```bash
cd project
git pull
git submodule update
```

方法二：先进入子模块，然后切换到需要的分支，在子模块中pull

```bash
cd project/moduleA
git checkout <branch-name>
cd ..
git submodule foreach git pull
```

### 5. 删除submodule

先unregister需要删除的子模块，之后可以`git-rm`移除子模块代码文件

```bash
cd project
git submodule deinit <submodule-name>
git rm <submodule-name>
git commit -m 'delete submodule <submodule-name>'
```

`--force`选项会强制删除子模块，即便子模块已经有本地修改

`.git/config`和`.gitmodules`中相应内容自动被删除。在之后的`git submodule`和`git submodule foreach`都会跳过unregister的子模块

## Reference

[Git Submodule管理项目子模块](https://www.cnblogs.com/nicksheng/p/6201711.html)

[Git中submodule的使用](https://zhuanlan.zhihu.com/p/87053283)

[Git Submodule使用完整教程](https://www.cnblogs.com/lsgxeva/p/8540758.html)