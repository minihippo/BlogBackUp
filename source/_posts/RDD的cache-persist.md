---
title: 'RDD的cache(), persist()和checkpoint'
date: 2018-07-09 16:58:11
tags:
    - spark
    - rdd
toc: true
---

将RDD缓存有利于后续action的操作。

#### RDD缓存
cache == persist(MEMORY_ONLY), 只缓存在内存中。
persist提供多个缓存程度的选择。

<!--more-->

<img src='/picture/rdd_persist.png', style='zoom:50%' />
缓存rdd使用hashmap存储，key为rdd.id，value为rdd内容，由blockManager管理，内存中memoryStore负责，磁盘中diskStore负责。
后续的resultTask会追溯所依赖的rdd是否已经cache过了，是则向blockManager发送请求，判断是否可以直接获取还是需要重新计算。

如果存储rdd的内存达到阈值？
spark会根据rdd缓存的程度进行写入磁盘或者删除。这些删除的rdd可以通过世代关系重新计算得到。

#### checkpoint
缓存有可能丢失，或者因内存不足而删除。对于丢失的部分RDD可以重计算得到。但是多次迭代后的数据丢失重计算开销会很大。因此需要即使缓存丢失，也能快速恢复。由此引入了checkpoint。
checkpoint：
- rdd会存储在hdfs中，保证容错
- persist之后rdd的依赖关系仍保留，而checkpoint之后rdd的依赖关系会丢掉
- checkpoint和persist一样，都是transformation，遇到action才会执行
- checkpoint的数据即使任务结束也不会消失，persist是会的
- checkpoint之前都要先cache，这是因为checkpoint需要把job重新从头算一遍，cache之后可以直接保存缓存重的rdd
