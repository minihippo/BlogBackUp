---
title: 'combineByKey, reduceByKey, aggregatedByKey等区别'
date: 2018-07-08 22:20:13
tags:
    - spark
    - rdd
    - transformation
    - 小记
toc: true
---

reduceByKey,aggregatedByKey都是基于combineByKey。groupByKey没有mapside combine。
~~~scala
def combineByKey[C](createCombiner: V => C, mergeValue: (C, V) => C, mergeCombiners: (C, C) => C) : RDD[(K, C)]

def aggregateByKey[U](zeroValue: U)(seqOp: (U, V) => U,combOp: (U, U) => U): RDD[(K, U)]

def reduceByKey(func: (V, V) => V): RDD[(K, V)]

def groupByKey(T => K): RDD[(K, Iterable[V])]
~~~

<!--more-->

**aggregateByKey和combineByKey区别**：aggregateByKey直接可以创建zeroValue，而不用根据v初始化combiner，当mergevalue和createCombiner操作相同时，优先使用aggregateByKey


#### 1.CombineByKey
源码
~~~scala
def combineByKey[C](
  createCombiner: V => C,
  mergeValue: (C, V) => C,
  mergeCombiners: (C, C) => C): RDD[(K, C)] = self.withScope {
combineByKeyWithClassTag(createCombiner, mergeValue, mergeCombiners)(null)
}
~~~

#### 2.aggregatedByKey
源码
~~~scala
def aggregateByKey[U: ClassTag](zeroValue: U, partitioner: Partitioner)(seqOp: (U, V) => U,
  combOp: (U, U) => U): RDD[(K, U)] = self.withScope {

// 中间代码省略，主要看最后一个，调用combineByKey

val cleanedSeqOp = self.context.clean(seqOp)
// seqOp，同时是，createCombiner，mergeValue。而combOp是mergeCombiners
combineByKeyWithClassTag[U]((v: V) => cleanedSeqOp(createZero(), v),
  cleanedSeqOp, combOp, partitioner)
}
~~~

#### 3.reduceByKey
源码
~~~scala
def reduceByKey(partitioner: Partitioner, func: (V, V) => V): RDD[(K, V)] = self.withScope {  
  combineByKey[V]((v: V) => v, func, func, partitioner)
}
~~~


