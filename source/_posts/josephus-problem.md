---
title: Josephus Problem
date: 2020-07-15 00:00:00
tags:
  - algorithms
mathjax: true
---


# Josephus Problem

## 问题描述

Josephus problem (or Josephus permutation), 简单说，N个人围成一圈，从某一人（比如第k个）开始报数，报到m的人退出，如此循环，直到最后一个人

> People are standing in a circle waiting to be executed. Counting begins at a specified point in the circle and proceeds around the circle in a specified direction. After a specified number of people are skipped, the next person is executed. The procedure is repeated with the remaining people, starting with the next person, going in the same direction and skipping the same number of people, until only one person remains, and is freed.

## 解法1: circular linked list

循环链表解法很直接，复杂度O(n)

每个Node就是问题中的一个人，数到谁就删除相应的Node，最后只剩一个Node

```java
class Node {
    int data;
    Node next;
    public Node(int data){
        this.data = data;
    }
}

public Node createCirLinkedList(int N) {
     Node head = new Node(1);
     Node next = head;
     for (int i = 2; i <= N; i++){
         Node tmp = new Node(i);
         next.next = tmp;
         next = next.next;
      }
      next.next = head;
      return head;
}

public int solve(int N, int m) {
     /* create circurlar linked list */
     Node head = createCirLinkedList(N);
     /* remove node recursively */ 
     while (head != head.next){
         /* move head point to the previous node before to-be-deleted node */
         /* head.next is the node to be deleted */
         for (int i = 1; i < m - 1; i++){
             head = head.next;
         }
         System.out.print(head.next.data+"->");
         /* remove the m-th node */
         head.next = head.next.next;
         head = head.next;
         }
         System.out.print("Done!\n");
         return head.data;
}
```

## 解法2: 数学方法递推公式

求Josephus的递推就比较有意思啦，正向不行，得反着来。所有人围成一个圆，整个序列从任意一点开始都可，0点（index是0）是谁都一样可以数下去。那么每轮都把上一次被删除的那个人的下一个位置作为新的0点，开始新一轮数数。这样子，每一轮都是重复一样的模式在数数，recursion来了。

$J(n)$表示n个人玩最后胜利者的编号（编号从0开始），按照这样设定初始条件$J(1) = 0$  

过程：

$J(n)$:

   编号m % ( n - 1 )出局，剩下的人组成一个新的环，从k = m % n开始

   0, 1, 2, 3, 4, ..., k-2, ~~k-1,~~ k, k+1, k+2, k+3, k+4, ..., n-1

$J(n-1)$:

   n-k, n-k+1, n-k+2,..., n-2,   0, 1, 2, 3, 4, ..., n-k-1,

   编号做一下转换：

            J(n)编号 --> J(n-1)编号
    
                   k --> 0
    
                 k+1 --> 1
    
                 k+2 --> 2
    
                     ...
    
                 k-2 --> n-2
    
                 k-1 --> n-1

n个人玩J(n)时的序号转换成n-1个人玩J(n-1)的序号，这样之，当我们知道子问题J(n-1）的胜利者编号就可以转换成J(n)时的编号，J(1)->J(2)->...->J(n-1)->J(n), 递推公式就来了

```
   J(n) = (J(n-1) + k) % n, k = m % n
=> J(n) = (J(n-1) + m % n) % n
        = (J(n-1) + m) % n
```

递推公式:
J(1) = 0
J(n) = (J(n-1) + m) % n

### recursive

```java
public int solve(int N, int m) {
     /* index begin at 0 */
     if (N == 1) {
         return 0;
     }
     return (recursiveSolve(N - 1, m) + m) % N;
}

/* when index begins at 1 */
int res = solve(N, m) + 1;
```

### iterative

```java
public int solve(int N, int m) {
     /* index begin at 0 */
     int res = 0;
     for (int i = 2; i <= N; i++){
         res = (res + m) % i;
     }
     /* when index begin at 1 */
     res = res + 1;
     return res;
}
```

## Reference

[Josephus problem wikipedia](https://en.wikipedia.org/wiki/Josephus_problem)

[Josephus Problem的详细算法](https://www.cnblogs.com/jclian91/p/8660123.html)
