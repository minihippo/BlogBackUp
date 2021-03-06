---
title: 数据库文件系统索引原理 b-树
date: 2018-02-09 15:04:33
tags:
    - 数据库
    - 索引
    - b树
    - 小记
toc: true
reward: true
---

阿里面试问到过。

## Reference

超详细[数据结构与算法（7）：数据库索引原理及优化](https://plushunter.github.io/2017/07/21/数据结构与算法（7）：数据库索引原理及优化/)

<!--more-->

## 索引原理
　　索引一种数据结构，以某种方式指向数据，它存储在磁盘上。为数据建索引是为了减少I/O访问次数，更有效的查找文件。

**二叉树**： 每个节点最多两个子树。查找一条数据复杂度是$\log n$。查找效率和层数有关，层数越深，比较次数越多。
对于数据库来说，每进入一层就需要从硬盘读取一次数据，因此层数限制了速度。
**平衡多叉树**：

| B树                | B+树               |
| ----------------- | ----------------- |
| \-                | 更能表示区间            |
| 中间节点存储数据          | 只有叶子结点存储数据，中间节点索引 |
| 子树m个，中间节点m\-1个关键字 | 子数m个，中间节点m个关键字    |


### 1. B树
**特性**：对于一个m阶B树
1. 根节点子树数量[2,M]
2. 分支节点子树数量[M/2,M]
3. 叶子节点位于同一层
4. 关键字数量满足$ceil(M/2)\-1 \leq n \leq m\-1$
5. 有k个子树的分支节点关键字数量为k-1

　　如图，3阶B树
<img src="/picture/b树-1.png", style="zoom:50%" />
**搜索方式**：
查找某个关键字$K$，先取根节点，根据根节点包含的关键字$K\_1\dots K\_n$找到是否含有$K$，或者确定关键字所在的区间$K\_i, K\_(i+1)$。再根据该区间所指向的子树继续查找，直到找到或子树为空。
若一个节点可以容纳100个值，那么3层B树可以存储100万条数据，查找目标值最多读两次硬盘。**减少定位记录时所经历的中间过程**

**b树的删除插入**：
删除插入操作必须满足b树的特性，尤其是2、4、5三点。具体的示例见最后。

### 2. B+树
　　B+树是B树的变体，中间节点不存储只作为索引，所有数据存储在叶子节点上。B+树更适合区间查询，因此更适合文件索引系统。
**特性**：（类似B树）
1. 有k个子树的分支节点关键字数量为k
2. 子树指针指向的区间和B树不同
3. 所有关键字存储在叶子节点上
4. 叶子节点的链表是有序的

　　如图，3阶B+树
<img src="/picture/b树-4.png", style="zoom:50%" />
数据库会在经典的B+树结构上进行优化，在叶子节点上增加顺序访问指针，提高区间的访问性能，如上图所示B\*树。

## MySQL 索引实现

##### MySQL 索引类型

| 类型   | 含义             |
| ---- | -------------- |
| 主键索引 | 特殊的唯一索引，不允许有空值 |
| 唯一索引 | 列值必须唯一，但允许空值   |
| 普通索引 | 无限制            |
| 组合索引 | 一个索引包含多个列      |
| 全文索引 | 全文检索           |

| 类型    | 含义                              |
| ----- | ------------------------------- |
| 聚集索引  | 索引按照数据存放的物理位置为顺序/叶节点就是数据节点      |
| 非聚集索引 | 索引的逻辑顺序和物理存储顺序不同/叶节点存储指向数据地址的指针 |

　　MyISAM与InnoDB的区别

| MyISAM                    | InnoDB               |
| ------------------------- | -------------------- |
| 叶节点：数据地址                  | 叶节点：数据块              |
| 非聚集索引                     | 聚集索引                 |
| 索引数据分离                    | 索引数据不分离              |
| 非必须主键                     | 必须要有主键               |
| 辅助索引叶节点存储数据地址             | 辅助索引叶节点存储主键          |
| 适用于大数目的不同值、频繁更新的列、频繁修改索引列 | 适用于返回某范围内的数据、小数目的不同值 |
|                           | 不适用于主键很长             |

###  1.MyISAM索引实现

　　MyISAM使用B+树作为索引结构，叶节点存储数据地址。
主索引
<img src="/picture/b树-6.png", style="zoom:50%" />
辅助索引
<img src="/picture/b树-7.png", style="zoom:50%" />
主索引和辅助索引结构上没有区别，主索引要求key唯一而辅助索引不用。

### 2.InnoDB索引实现
　　InnoDB使用B+树作为索引结构，叶节点存储数据块。
主索引
<img src="/picture/b树-8.png", style="zoom:50%" />
辅助索引
<img src="/picture/b树-9.png", style="zoom:50%" />
主索引因为InnoDB数据文件本身要按照主键聚集所以必须要有主键。
辅助索引记录的是主键的值而不是数据地址。因此需要检索两边索引：先获得主键再获得记录。




### 索引使用策略

　　组合索引的**最左前缀匹配原则**：对于多列索引，总是从索引最前面字段开始匹配，接着往后/向右，中间不能跳过。（组合索引的每个字段都是有序的，先按A字段排序再B字段排序）

~~~sql
select * from table where a=1 and b=2 and c=3；  //用到了索引a、b、c
select * from table where a=1 and c=3；    //只用到了索引a
~~~

因此在创建多列索引时，where子句中使用最频繁的一列放在最左边

### 索引的缺点

- 对于数据量大的，插入删除数据时索引需要更新
- 索引需要额外空间
- 查询索引也需要时间


#### B树插入删除
　　对于一个3阶树
**插入**：
<img src="/picture/b树-2.png", style="zoom:50%" />

**删除**：
<img src="/picture/b树-3.png", style="zoom:40%" />

