---
title: Segment Tree
tags:
	- Data Structure
---

# Segment Tree

线段树呢，首先是树状的数据结构，每个节点代表一个区间或线段。以二叉树的形式来储存一区间上的某一特性（像区间和，区间里的最小值等）。线段树的操作都基于递归和回溯。

1. 线段树是一种二叉搜索树，将一个区间划分成一些单元区间，每个单元区间对应线段树中的一个叶节点
2. 对于线段树中的非叶子节点[a, b]， 左子节点表示的区间为[a, (a+b)/2]，右子节点表示区间[(a+b)/2, b]
3. 线段树最后的节点总数为$2^{\lceil log_2{n} \rceil + 1}$，n为区间总长度

## 线段树的操作

### 点更新



### 区间查询



### 区间更新



## Java版本实现

关于存储用的数组空间开到4*N（N为区间长度）：



## Reference

[线段树详解](https://www.cnblogs.com/xenny/p/9801703.html)

[夜深人静写算法（七）- 线段树](https://blog.csdn.net/whereisherofrom/article/details/78969718)

[浅谈线段树（Segment Tree）](https://wmathor.com/index.php/archives/1175/)

[线段树详解（原理，实现与应用）](https://blog.csdn.net/zearot/article/details/48299459#t26)

[线段树总结-java版](https://blog.csdn.net/xushiyu1996818/article/details/90293988)

[Segment Tree, CP-Algorithms](https://cp-algorithms.com/data_structures/segment_tree.html)