---
title: 'Spark job, stage,task划分与提交'
date: 2018-06-09 18:29:44
tags:
    - Spark
    - 小记
---

进入8080端口看到Spark任务管理也页面，可以看到我们提交的任务是有一个Application ID。点进去会分成多个Job ID，点进一个job又有多个stage，stage点进去就是executor和task的详细情况。
显然，Application > Job > Stage > Task。

<!--more-->

### 划分原理

**job每遇到action操作就进行分割。stage每遇到宽依赖就进行分割。**
Spark中有两类task：shuffleMapTask和resultTask，前者输出shuffle前所需结果，后者输出result。每遇到一个宽依赖，shuffle前的所有变换是一个stage，shuffle操作及后续是一个stage。如果一个job中有多次shuffle，那么每个shuffle之前都是一个stage。

~~~shell
job1 = stageA + stageB (+ stageC)
stageA == 多个shuffleMapTask
stageB == resultTask (+ shuffleMapTask)
(stageC == resultTask) 
~~~

**然而当遇到sortBy、zipWithIndex等操作也会生成一个job**，它们是transformation，这个是为什么呢？
Ans：Spark在运行sortByKey之前会先进行sample，确定key的range和分布情况，确保后续真正的排序操作每个partition的数据分布均匀。

### 详细流程
**DAGScheduler负责stage的调度管理。TaskScheduler负责task的调度管理。**DAGScheduler还处理由于shuffle 数据丢失导致的失败，可能需要重新提交运行之前的stage。非shuffle数据丢失导致的task失败由TaskScheduler处理。 

1. 当任务提交到Driver端，每遇到一个action操作，触发SparkContext的`runJob`方法
2. SparkContext调用DAGScheduler的`runJob`方法，`runJob`提交`JobSubmitted`的消息
3. DAGScheduler接收消息，把job分为多个stage
4. DAGSchedule把stage以taskSet的形式提交给TaskScheduler
5. 通过TaskScheduler把taskSet添加到任务队列当中，交给SchedulerBackend进行资源分配和任务调度
6. 调度器给Task分配执行Executor，ExecutorBackend负责执行Task

#### 划分stage
3.DAGScheduler接收消息，把job分为多个stage
- DAGSchedule根据最后一个Rdd（即action操作RDD）生成finalStage
- 生成finalStage的过程中，从最后一个RDD开始向上遍历其父RDD。若遇到父RDD为窄依赖，则压栈；若为宽依赖，则生成该shuffle过程所在的stage。
- 完成栈中Rdd遍历后，所有的stage划分完成。

#### 提交stage
1. DAGScheduler获取finalStage的父stage
3. 如果存在父stage，则将该调度阶段存储在waitingStages列表中，同时递归提交父stage
4. 如果不存在父stage,则直接执行该stage的所有task
5. 当一个stage执行完成后，检查其任务是否全部完成。如果完成，则执行waitingStage列表中的stage，直到所有调度阶段都执行完毕

#### 关于Task
executor是application运行在worker上的一个进程，一个worker可能有多个executor。
task是运行在executor上的工作单元，是application最小的处理单元。
提交过程：
4.DAGSchedule把stage以taskSet的形式提交给TaskScheduler
- TaskScheduler会生成TaskSetManager单独管理一组taskSet，task的失败次数等等
- TaskScheduler中的schedulableBuilder有种调度策略FIFOSchedulerBuilder和FairSchedulerBuilder，它根据调度策略决定TaskSetManager的执行顺序


以上，部分通过阅读源码了解，task相关通过blog了解。
