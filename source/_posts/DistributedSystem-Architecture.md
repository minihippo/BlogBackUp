---
title: DistributedSystem_Architecture
date: 2017-10-01 14:13:56
tags: 
     - 分布式系统
     - 笔记
toc: true
reward: true
---
# 2. Architectures

*Terminology*:

* Component:  a modular unit with well-defined interfaces and is replaceable within its environment.
* Connector: mediating communication,coordination or cooperation among components

<!--more-->

Architecture style is formulated in terms of components, the way that components are connected to each other, the data exchanged between components. 

### 2.1 Architecture Styles

*Divided into*:$^1$ layered architecture, $^2$objected-based architecture, $^3$data-centered architecture, $^4$event-based architecture.

* Layered architecture   

  used for client-server system, the most important.

  <img src="/pic/layered.png" width="300" />

* Objected-based architecture   

  used for distributed object systems, processes communication by method call, the most important.
   
  <img src="/pic/objected-based.png" width="300" />

* Data-centered architecture   

  repository for process communication,  decoupled in space.

  <img src="/pic/shared-data.png" width="300" />

* Event-based architecture 

  process communicated by event propagation. 

  The following is shared data space architecture. It is combined by data-centered and event-based architectures, decoupled in space and time.

  <img src="/pic/event-based.png" width="300" />

### 2.2 System Architecture 

*Definition*: an **instance** of a software architecture which realizes $^1$the ~~logical~~ software component, $^2$their ~~logical~~ interaction, and $^3$their physical placement.

*Types*: centralized architectures, decentralized architectures, hybrid architecture.

#### 2.2.1 Centralized Architecture

Basic Client-Server Model: a single server(physical) implements most of the software components while remote clients can access that server.

*Characteristics*:

* processes in servers offer services.   //single point of failure,bottleneck,scaling difficult
* process in clients user services. (processes in a distributed system divided into the metioed two groups)
* clients and servers can be on different machines.
* clients follow requests/reply model when using services.

<img src="/pic/clientservermodel.png" width="500" />

*Expension:* cause single point or multi points sufferred pressure

* proxy server: as an intermediate node betwwen servers and clients
* applets: clients download applet from servers, and check on local system

##### 2.2.1.1 Application Layering

*Traditional three-layered view:*

* User-interface layer
* Processing layer（处理层）
* Data layer

<img src="/pic/threelayerapplication.png" width="500" />

For instance, internet search engine.

<img src="/pic/threelayergoogleinstance.png" width="500" />

In general, it is possible to distribute c/s model into multi physical machines.

##### 2.2.1.2 Multitiered Architectures

Pre: single-tiered: dumb terminal/mainframe, dumb client do nothing.

Two-tiered: client/single server, like the following pic(thin->fat client)

<img src="/pic/twotiered.png" width="500" />

Three-tiered: each layer on separate machine. As 2.2.2.1 mentioned, application layer can be on an extra physical machine.  

In a word, all the mentioned above is belong to vertical distribution, different layers distributed on different machines.

#### 2.2.2 Decentralized Architecture

A logical server may be physically split up into logically equivalent parts(balancing the load). 

It is belong to horizontal distribution.

peer to peer system:

* *Types divided by overlay network style*: structured, unstructured. 
* Data is routed over connections setup between the nodes.

##### 2.2.2.1 Structured Peer-Peer

*Organization:* nodes are organized following a specific distributed data structure/topology. The protocol ensures any node can search the network for a resource.

*Examples*: chord system, the nodes in a structured overlay network such as a logical ring (created by Distributed Hash Table DHT) and make specific nodes responsible for services based only on their ID. 

##### 2.2.2.2 Unstructured Peer-Peer

*Organization*：nodes are organized as a random overlay ntework. Each node maintains a list of neighbors which are randomly chosen live node from the current set of nodes.

Flooding: node u sends a lookup query to all of its neighbors.

##### 2.2.2.3 Superpeers

An expansion of peer-to-peer system.

*Characteristics*: select a few nodes to do specific work

* Superpeers maintain an index for search
* Superpeers are able to setup connections
* Regular peers connected as a client to a superpeer. All communication from and to a regular peer proceeds through that peer's associated superpeer.

<img src="/pic/superpeers.png" width="300" />

#### 2.3 Hybrid System Architecture

Client-server combined with P2P

* Edge-server architectures, used for content delivery networks

  <img src="/pic/edge-serversystem.png" width="600" />

* Collaborative distributed system, bit torrent

  <img src="/pic/bittorrent.png" width="600" />

