---
title: Git Pattern
tags:
	- Git
---

## Mirroring a repository

1. create a bare clone of the repository
```bash
$ git clone --bare https://github.com/exampleuser/old-repository.git
```
2. mirror-push to the new repo
```bash
$ cd old-repository.git
$ git push --mirror https://github.com/exampleuser/new-repository.git
```
3. remove the temporary local repo created earlier
```bash
$ cd ..
$ rm -rf old-repository.git
```

## 修改完代码，发现分支错了

前提：尚未将修改提交到错误的分支

```bash
$ git stash

$ git checkout <target-branch>
$ git stash pop
$ git add <to-be-committed-files>
$ git commit -m 'xxx'
```

