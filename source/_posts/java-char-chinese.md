---
title: java_char_chinese
tags:
  - Java
date: 2021-04-01 23:27:00
---


# Java中的char类型和中文字符

开篇发问

```
Java里的char类型能不能存储一个中文字符？
```

答案是：可以，但细究起来，并不是简简单单的可以

### Unicode

对于中文来说，编码方式的使用字节大小各有不同。utf-8：一个中文占用三个字节，utf-16：一个中文占2个字节；gbk(中国人的编码方式)一个汉字2个字节等。Unicode类似一个文字容器，编码方式是一个解码工具，目的是在unicode的字符集中寻找一个对应的字符(我的理解是编码方式是快递员)。众所周知，Unicode有3种编码方式：utf-8, utf-16, utf-32。Unicode标准把代码点分成了17个代码平面（Code Plane），编号为#0到#16。每个代码平面包含65,536（2^16）个代码点（17*65,536=1,114,112）。其中，Plane#0叫做基本多语言平面（Basic Multilingual Plane，BMP），其余平面叫做补充平面（Supplementary Planes）。

下面是这些平面的名字和用途：

Plane#0 BMP（Basic Multilingual Plane）大部分常用的字符都坐落在这个平面内，比如ASCII字符，汉字等。
Plane#1 SMP（Supplementary Multilingual Plane）这个平面定义了一些古老的文字，不常用。
Plane#2 SIP（Supplementary Ideographic Plane）这个平面主要是一些BMP中没有包含汉字。
Plane#14 SSP（Supplementary Special-purpose Plane）这个平面定义了一些非图形字符。
Plane#15 SPUA-A（Supplementary Private Use Area A）
Plane#16 SPUA-B（Supplementary Private Use Area B）



### Java中char类型

Java中的char类型是按照Unicode规范实现的一种数据类型，固定16bit大小。现如今，Unicode字符集已经进行了扩展，表示的范围已经超过了16bit。Unicode字符集的数值范围扩大到了[U+0000,U+10FFFF]。一个char值可以表示BMP范围内的Unicode字符。BMP表示[U+0000, U+FFFF]之间的Unicode字符。而且，绝大部分的中文字符的Unicode范围是[0x4E00, 0x9FBB],恰好是在BMP范围内。



就常用的UTF-8编码来说，我们都听说过他是用3或者4个字节来表示一个汉字的。就拿3个字节来算的话，一个char也存不下？？

UTF-8编码和代码点对应关系:

|   Unicode编码   |             UTF-8字节流              |
| :-------------: | :----------------------------------: |
| 000000 - 00007F |               0xxxxxxx               |
| 000080 - 0007FF |          110xxxxx 10xxxxxx           |
| 000800 - 00FFFF |      1110xxxx 10xxxxxx 10xxxxxx      |
| 010000 - 10FFFF | 11110xxx 10xxxxxxx 10xxxxxx 10xxxxxx |

当一个字节表示一个字符时，二进制开头是0；当两个字节表示一个字符时，二进制开头是11；当3个字节表示一个字符时，二进制开头是111。UTF-8编码加入了多余的标识位来区分一个Unicode代码点！才会出现中文汉字集中在[0x4E00, 0x9FBB]范围的16bit数值内，UTF-8却需要3个字节存储的原因。

那么UTF-8占用3到4个字节，char只能存2个字节（16bit），然而UTF-8中的几乎所有汉字都是在BMP范围内，也就是在char可存储的范围内，是不是矛盾了？

其实不然，

1. char字符存储的是Unicode编码的代码点，也就是存储的是U+FF00这样的数值，然而我们在调试或者输出到输出流的时候，是JVM或者开发工具按照代码点对应的编码字符输出的，Java内部使用UTF-16编码。

2. 所以虽然UTF-8编码的中文字符是占用3个或者4个字节，但是对应的代码点仍然集中在[0x4E00, 0x9FBB]，所以char是能够存下在这个范围内的中文字符的。
3. 但是对于超过16bit的Unicode字符集，也就是Unicode的扩展字符集，一个char是放不下的，需要两个char才能放下。



另外，如何判断java字符串是否包含中文？

```java
/**
 * 判断某个字符是否为汉字
 *
 * @param c 需要判断的字符
 * @return 是汉字返回true，否则返回false
 */
public static boolean isChinese(char c)
{
    String regex = "[\\u4e00-\\u9fa5]";
    return String.valueOf(c).matches(regex);
}
```

这里没有考虑CJK扩展字符集。



PS: 一个查找中文字符utf-8编码网址：http://www.mytju.com/classcode/tools/encode_utf8.asp

## reference

[Java中的char究竟能存中文吗？](https://www.cnblogs.com/softidea/p/10271219.html)

[细说Java中的字符和字符串（一）](https://blog.csdn.net/buqutianya/article/details/80685437)

[HanLP自然语言处理库](https://github.com/hankcs/HanLP/blob/master/src/main/java/com/hankcs/hanlp/utility/TextUtility.java)