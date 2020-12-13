---
title: 康托展开
tags:
  - algorithms
date: 2020-12-13 10:01:43
---


# 康托展开

简单说：

给出一个1-n的全排列 -> 问这是第几个排列(字典序), 这是康托展开

知道全排列长度和字典序 -> 问这个全排列长啥样，这是逆展开

来自[Wikipedia](https://zh.wikipedia.org/wiki/%E5%BA%B7%E6%89%98%E5%B1%95%E5%BC%80)的说明：**康托展开**是一个全排列到一个自然数的双射，常用于构建哈希表时的空间压缩。 康托展开的实质是计算当前排列在所有由小到大全排列中的顺序。

把1-n的所有排列按字典序排序，这个排列的位次就是它的排名。排列和它的排名是一一对应的，也就是说康托展开是双射，正反过程都是行得通的，才有逆展开。例子可以看[LeetCode 60.排列序列](https://leetcode-cn.com/problems/permutation-sequence/)的问题描述，对字典序和逆过程可以直观理解一下。



## 简单展开

$X = a_n(n-1)! + a_{n-1}(n-2)! + ... + a_10!$

一个全排列，第i位有n-1-i种选择。

比如4321这个全排列，第一个数为4，比4小的选择有{1, 2, 3} 3个数

```
0   1 2 3 4
1   1 2 4 3
2   1 3 2 4
3   1 3 4 2
4   1 4 2 3
5   1 4 3 2
6   2 1 3 4
7   2 1 4 3
8   2 3 1 4
9   2 3 4 1
10  2 4 3 1
11  2 4 3 1
12  3 1 2 4
13  ...
```

考虑到后面还有3位（n=3的全排列有3!种），所以开头为4的排列的序号从3\*3!开始。首位为k的n阶全排列，它表示的数在从0开始第k个长为(n-1)!的区间，这个区间为[(k-1)\*(n-1)!, k\*(n-1)!]

```
0   X 2 3 4  <==>  1 2 3
1   X 2 4 3  <==>  1 3 2
2   X 3 2 4  <==>  2 1 3
3   X 3 4 2  <==>  2 3 1
4   X 4 2 3  <==>  3 1 2
5   X 4 3 2  <==>  3 2 1
6   X 1 3 4  <==>  1 2 3
7   X 1 4 3  <==>  1 3 2
8   X 3 1 4  <==>  2 1 3
9   X 3 4 1  <==>  2 3 1
10  X 4 3 1  <==>  3 1 2
11  X 4 3 1  <==>  3 2 1
12  X 1 2 4  <==>  1 2 3
13  ...
```

上图展示了4阶全排列和3阶全排列的关系，也就是说，去掉第1位后，4阶全排列可以转化为3阶全排列

公式中的$a_n$可以理解为第n位的数字排在还没使用的数字中第几位（从0开始），或者以$a_n$为前项的逆序对的个数。$a_n$组成的序列也被叫做Lehmer Code。

例子：

2314 -> 1100

14523 -> 02200

得到的逆序对序列可以当成一个特殊进制的数（第n位对应(n-1)!权重），转换成10进制就是要求的全排列的字典序（别忘了+1）

```java
    public static int getOrder(int n, String permutation) {
        int[] num = new int[n];
        for (int i = 0; i < n; i++) {
            num[i] = Integer.valueOf(permutation.charAt(i));
        }
        int res = 0;
        for (int i = 0; i < n; i++) {
            int inverse = 0;
            for (int j = i + 1; j < n; j++) {
                if (num[i] > num[j]) {
                    inverse++;
                }
            }
            res = res * (n - i) + inverse;
            // res += res + inverse * factorial[n - 1 - i];
        }
        return res + 1; // start at 1
    }
```

## 逆展开

逆展开就是上面字典序到全排列的过程，利用进制转换的代码完成字典序到逆序对序列（相当于10进制转换到以n!为进制的数），在根据逆序个数还原全排列（每次剩下还没使用的数字集合都在改变！）

```java
    public String getPermutation(int n, int k) {
        List<Integer> bits = new ArrayList<>();
        for (int i = 1; i <= n; i++) {
            bits.add(i);
        }
        int[] factorial = { 1, 1, 2, 6, 24, 120, 720, 5040, 40320, 362880 };
        StringBuilder sb = new StringBuilder();
        int residue = k - 1;
        for (int i = 0; i < n; i++) {
            int iid = residue / factorial[n - 1 - i];
            residue = residue % factorial[n - 1 - i];
            sb.append(bits.get(iid));
            bits.remove(iid);
        }
        return sb.toString();
    }
```


## 升级版展开

简单版本康托展开使用一个循环来数逆序个数，整体代码的复杂度是O(n^2)。可以改进的地方就在如何更快地数逆序，一种办法可以用分治类比merge sort的代码来计算逆序，复杂度为O(logn)；另外也可以用线段树或者树状数组来统计逆序，复杂度同样为O(logn)。







## Reference

[迟到的【洛谷日报#187】浅谈康托展开](https://zhuanlan.zhihu.com/p/98524232)

[【给初心者的】康托展开](https://zhuanlan.zhihu.com/p/39377593)