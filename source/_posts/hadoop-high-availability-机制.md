---
title: hadoop high availability 机制
date: 2018-06-01 22:03:05
tags:
    - 小记
    - hadoop
    - namenode
    - zookeeper
---

为了解决namenode单点失效问题，提出namenode双机备份，使namenode失效后能直接切换到备份节点，且不存在数据一致性问题。

### Overview
　　架构
![architecture](/picture/hadoop-ha-1.png)

<!--more-->

#### namenode同步
- 两个namenode都与一组称为JNS(Journal Nodes常为奇数个)的互相独立的进程保持通信。
-当Active Node上更新了namespace，它将记录修改日志发送给JNS的多数派。
- Standby noes将会从JNS中读取这些editslog，并持续关注它们对日志的变更。Standby Node将日志变更应用在自己的namespace中。
- 当failover发生时，Standby将会在提升自己为Active之前，确保能够从JNS中读取所有的editlogs。


#### namenode切换

　　流程
![procedure](/picture/hadoop-ha-2.png)

**ZKFailoverController**:作为 NameNode 机器上一个独立的进程启动，启动的时候创建 HealthMonitor 和 ActiveStandbyElector 这两个主要的内部组件。
**HealthMonitor**: 负责检测 NameNode 的健康状态，如果 NameNode 的状态发生变化，告诉ZKFailoverController 进行自动的主备选举。 
**ActiveStandbyElector**： 主要负责完成自动的主备选举，内部封装了 Zookeeper 的处理逻辑，一旦 Zookeeper 主备选举完成，会回调 ZKFailoverController 的相应方法来进行 NameNode 的主备状态切换。

具体流程：
　　集群启动后一个NameNode处于Active状态，并提供服务，处理客户端和DataNode的请求，并把editlog写到本地和share editlog（这里是QJM）中。另外一个NameNode处于Standby状态，它启动的时候加载fsimage，然后周期性的从share editlog中获取editlog，保持与Active节点的状态同步。
　　为了实现Standby在Active挂掉后迅速提供服务，需要DataNode同时向两个NameNode汇报，使得Standby保存block to DataNode信息，因为NameNode启动中最费时的工作是处理所有DataNode的blockreport。
　　为了实现热备，增加FailoverController和Zookeeper，FailoverController与Zookeeper通信，通过Zookeeper选举机制，FailoverController通过RPC让NameNode转换为Active或Standby


##### Reference
2版本以后的更新
[hadoop document about HA](hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HDFSHighAvailabilityWithNFS.html)

