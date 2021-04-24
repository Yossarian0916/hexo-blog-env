---
title: 蓄水池抽样
tags:
  - Algorithms
date: 2021-04-24 18:09:00
mathjax: true
---


# 蓄水池抽样

**给定一个数据流，数据流长度N很大，且N直到处理完所有数据之前都不可知，请问如何在只遍历一遍数据（O(N)）的情况下，能够随机选取出k个不重复的数据。**

算法应用的场景特点：

1. online算法，数据流一遍输入一遍抽样
2. 时间复杂度O(N)
3. 每个样本抽中概率=**k/N**，（k是事先确定的，与N无关）$^1$



### 抽样过程:

- 假设数据序列的规模为 n，需要采样的数量的为 k。

- 首先构建一个可容纳 k 个元素的数组，将序列的前 k 个元素放入数组中。

- 然后从第 i = k+1 个元素开始，生成一个随机数 d∈[1,i]， 如果 d <= k， 那么蓄水池的第 d 个元素被替换为数据流中的第 i 个元素

```java
int[] reservoir;

public void sample(int[] dataStream) {
    for (int i = 0; i < reservoir.length; i++) {
        reservoir[i] = dataStream[i];
    }

    for (int i = k; i < dataStream.length; i++) {
        int d = rand.nextInt(i + 1);
        if (d < k) {
            reservoir[d] = dataStream[i];
        }
    }
}
```



### 算法正确性：

1. 对于第 i 个数（i ≤ k）。在 k 步之前，被选中的概率为 1。当走到第 k+1 步时，被 k+1 个元素替换的概率 = k+1 个元素被选中的概率 x i 被选中替换的概率，即为 $\frac{k}{k+1}\frac{1}{k}=\frac{1}{k+1}$。则被保留的概率为 $1-\frac{1}{k+1}=\frac{k}{k+1}$。依次类推，不被 k+2 个元素替换的概率为 $1-\frac{k}{k+2}\frac{1}{k}=\frac{k+1}{k+2}$。则运行到第 n 步时，被保留的概率 = 被选中的概率 x 不被替换的概率，即：
$$1\frac{k}{k+1}\frac{k+1}{k+2}\frac{k+2}{k+3}...\frac{N-1}{N} = \frac{k}{N}$$

2. 对于第 j 个数（j > k）。在第 j 步被选中的概率为 $\frac{k}{j}$。不被 j+1 个元素替换的概率为 $1 - \frac{k}{j+1}\frac{1}{k} = \frac{j}{j+1}$。则运行到第 n 步时，被保留的概率 = 被选中的概率 x 不被替换的概率，即：
$$\frac{k}{j}\frac{j}{j+1}\frac{j+1}{j+2}\frac{j+2}{j+3}...\frac{N-1}{N} = \frac{k}{N}$$

所以对于其中每个元素，被保留的概率都为 $\frac{k}{N}$.




### 题外

抽样证明过程看起来和Knuth-shuffle算法有点像

```java
public static <T> void shuffle(T[] A) {
    for (int i = A.length - 1; i > 0; i--) {
        int rand = (int) (Math.random() * (i + 1));
        // swap A[i] and A[rand]
        T temp = A[i];
        A[i] = A[rand];
        A[rand] = temp;
    }
}
```



## reference

[蓄水池抽样算法（Reservoir Sampling）](https://www.jianshu.com/p/7a9ea6ece2af)

[【算法34】蓄水池抽样算法 (Reservoir Sampling Algorithm)](https://www.cnblogs.com/python27/p/Reservoir_Sampling_Algorithm.html)

[蓄水池采样算法](https://www.cnblogs.com/snowInPluto/p/5996269.html)



$^1$ 如果k与N有关呢？ 例如k=N/3，怎么抽样，思考一下

