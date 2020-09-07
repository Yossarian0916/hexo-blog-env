---
title: "Java IO"
date: 2020-04-01 00:00
tags:
	- Java
---



# Java I/O

I/O流 概念： 流数据从一端传到另一端

Java I/O流分为三大类：

+ 按照读写单元（bit or byte）：字节流，字符流
+ 按照流的方向：输入流，输出流
+ 按照功能分：节点流，处理流

字节流和字符流：

- 当我们读取文件然后再写入其他文件时，不用过多的关注读取的内容时，通常使用的是字节流，因为这相当于是在处理二进制文件。读取数据效率高，并且保持数据的完整性；当我们读取文件的内容，并对文件的内容进行加工时，通常使用字符流。

节点流和处理流：

- 节点流：即从特定的数据源读取写入数据
- 处理流：在已经存在的节点流或者处理流上，进行装饰提供更强大的读写功能
  1. 缓冲流（Buffered）带缓冲区，在节点流上添加缓冲区（例如：`new BufferedInputStream(new FileInputStream())`），这样避免读取文件时，大量进行硬盘的读写，提高读写效率
  2. 转换流（StreamReader）即字节流和字符流的相互转换，比如：在进行读写字节流时，想调用读取字符流的函数，就可以通过转换流。`InputStreamReader in = new InputStreamReader(new InputStream())` **注意只能字节流=>字符流**
  3. 数据流（Data）当读取写入具体的数值数据时，`DataInputStream`可以让你从`InputStream`读取Java基本类型来代替原始字节。如果读取的数据是由大于一个字节的Java基本类型构成，如int，long，float，double等，那么用`DataInputStream`是很方便的。读取的时候记住先读先写的原则，顺序不能乱，因为Java基本类型都是大于一个字节的，顺序不对，读取出来会出现乱码。所以在用`DataInputStream`的时候要么为文件中的数据采用固定格式，要么将额外信息保存到文件中，以便解析的时候确定数据位置和类型使用。






### Java IO类结构
**`InputStream, OutputSteam, Reader, Writer`都是抽象类**

贴张图：

![](../images/java_IO.jpeg)

## References

[Java流机制详解](https://blog.csdn.net/qq_16558621/article/details/51377887)
[Java IO流学习总结一：输入输出流](https://blog.csdn.net/zhaoyanjun6/article/details/54292148)

