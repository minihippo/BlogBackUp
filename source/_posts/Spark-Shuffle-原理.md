---
title: Spark Shuffle 原理
date: 2018-06-05 20:11:19
tags: 
    - Spark
    - 小记
    - Shuffle
toc: true
---

Shuffle阶段涉及序列化反序列化、跨节点网络IO以及磁盘读写IO等，代价很高。
**shuffle write**：每个map task将计算结果分成多份，每一份对应到下游stage的每个partition中，并且临时写到磁盘。
**shuffle read**：reduce task通过网络拉取map task的指定分区结果数据。

<!--more-->

## Spark Shuffle 实现演进

#### Hash Shuffle
1.1版本以前实现方式，2.0版本已淘汰
更早版本的Hash Shuffle 每个map task都会为每个partition写一个文件。假设有n个partition，一个executor上有m个map task，那么就会有n\*m个文件。生成文件时需要申请文件描述符，当partition很多时，并行化运行task时可能会耗尽文件描述符、消耗大量内存。因此后来Hash Shuffle进一步变成了如下版本。
![](/picture/shuffle-hash.png)
为了减少临时文件数，一个core上所有map task的相同分区的文件会进行合并，这样最多只有n个文件。但是当partition个数很多时还是没解决问题，容易OOM。

### Sort Shuffle
借鉴了Hadoop Shuffle的处理方式，对中间结果进行了排序。
![](/picture/shuffle-sort.png)
- 在map阶段，会先按照partition id、每个partition内部按照key对中间结果进行排序。所有的partition数据写在一个文件里，并且通过一个索引文件记录每个partition的大小和偏移量。这样并行运行时每个core只要2个文件，一个executor上最多2m个文件。。
- 在reduce阶段，reduce task拉取数据做combine时不再使用`HashMap`而是`ExternalAppendOnlyMap`。如果内存不足会写次磁盘。
排序会导致性能损失。

**Unsafe Shuffle**
为了进一步的优化内存和CPU使用，提升spark性能，引入了Unsafe Shuffle。
将中间结果以二进制的方式存储，直接在序列化的二进制数据上排序而不是java对象。排序的不是内容本身，而是内容序列化后字节数组的指针(元数据)，把数据的排序转变为了指针数组的排序，实现了直接对序列化后的二进制数据进行排序。既减少了内存使用和GC开销，也避免了频繁的序列化很反序列化。
限制：shuffle阶段不能有aggregate操作，分区数不超过$2^{24} \- 1$

Sort Shuffle包括Unsafe Shuffle，当条件符合就优先使用Unsafe Shuffle。

## Shuffle详细实现流程
针对SortShuffle的实现，具体解释整个实现流程。

### Shuffle Write
Shuffle Write 主要分为三个阶段：
- map的输出结果会写入`PartitionedAppendOnlyMap`相当于一个缓冲区，当这部分的内存写满了就会往磁盘溢写。
- 溢写称之为spill操作。在往磁盘溢写之前，会对数据进行排序，然后溢写。每个临时文件spill都会被记录在一个数组里
- 当一个map task执行完毕之后，对所有的临时文件和内存中的结果进行merge操作，做归并排序，然后输出最终结果并且生成索引文件。

关于磁盘溢写：
如果当前使用内存超过5M（`spark.shuffle.spill.initalMemoryThreshold`设置大小，默认5M，主要是防止spill的文件过小），map task会向`shuffleMemoryManager`申请`2 * currentMemory - myMemoryThreshold`大小的内存，如果申请不到就会触发spill操作。
`shuffleMemoryManager`是executor上所有task共享的， 并行task共享executor的内存，executor能分配出去的内存是`executorHeapMemory * spark.shuffle.memoryFraction * spark.shuffle.safetyFraction`。由于内存检查`estimateSize`是使用采样算法来进行估算的，采样周期不固定，呈指数增长。因此内存开的越大反而容易OOM，可以适当调低`spark.shuffle.safetyFraction`参数。

因此spark内存使用是很激进的，如果中间结果内存放得下会放进内存，会不会写磁盘做容错？

### Shuffle Read



### Reference
Spark Shuffle的演变，[Spark Shuffle原理及相关调优](http://sharkdtu.com/posts/spark-shuffle.html)
[Unsafe Shuffle实现流程](http://www.cnblogs.com/dt-zhw/p/5734921.html)
