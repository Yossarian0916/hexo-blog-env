---
title: Vim basic
tags:
  - vim
date: 2021-01-12 22:53:40
---


1. vim系统剪切板
"+y 复制到系统clipboard
"+p clipboard粘贴到vim
在Ubuntu中使用vim, 发现无法使用系统clipboard，可能是缺少相关的包
```bash
sudo apt-get install vim-scripts vim-gtk
# 检测vim是否支持system clipboard
vim --version | grep clipboard
```

2. Vim寄存器
    `:help register`查看10类48个寄存器
  - a-z命名寄存器（26个）
  - `"*, "+, "-` 选取拖放
  - `":` 只读，vim命令行
  - `"/` 搜说模式寄存器



## Reference:

[使用Vim寄存器](https://blog.csdn.net/a3192048/article/details/88290444)

