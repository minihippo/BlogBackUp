---
title: DistributedSystem_Introduction
date: 2017-09-27 20:26:20
tags: 
     - 分布式系统
     - 笔记
toc: true
reward: true
---
# 1.Introduction

### 1.1 Definition

*Distributed System*: a collection of autonomous computing elements that appears to users as a single coherent system.

<!--more-->

*Characteristics*:
  * **Hidden**: difference between computers and the way they communicate are hidden from users.
  * **Consistent & Uniform**: users interact with it in a consistent and uniform way,regardless of where and when.  
  * Easy to expand
  * Continuously avaliable

*As Middleware*:

![middleware](/pic/middleware.png)

### 1.2 Goals

**resources available, transparency, openness, scalablility**

#### 1.2.1 Resources Available

Users get resources conveniently.

#### 1.2.2 Transparency

*Definition*:  
- progress and resources sperated on multi-computers is hidden
- present itself to users and applications as if a **single** computer system.

*Type*:

| Transparency | Description                              |
| ------------ | ---------------------------------------- |
| Access       | Hide differences in data representation and how a resource is accessed |
| Location     | Hide where a resource is (implemented by using logical naming ) |
| Migration    | Hide that a resource move to another location (not affect how a resource is acessed) |
| Relocation   | Hide that a resource move to another location while in user |
| Replication  | Hide that a resource is replicated       |
| Concurrency  | Hide that a resource is shared by several competitive users |
| Failure      | Hide the failure and recovery of a resource |

#### 1.2.3 Openness

*Definition*: $^1$a system that offers services according to standard rules that $^2$ describe the syntax and semantics of those services.
* Well-defined interface
* Portability of applications
* Easy interoperability (互操作)

#### 1.2.4 Scalability

*Type*:
* Size scalability: number of users and processes
* Geographical scalability: maxinum distance between nodes
* Administrative scalability: easy to manage number of administrative domains

### 1.3 Types of Distributed Systems

Distributed  computing systems, distributed information systems, distributed pervasive systems.

#### 1.3.1 Distributed Computing System

##### 1.3.1.1 Cluster Computing  

* a group of high-end systems connected through a LAN
* homogeneity 同构: OS, hardware
* single managing node

##### 1.3.1.2 Grid Computing

* dispersed across several domains
* heterogeneous
* easily span a wide-area network

#### 1.3.2 Distributed Information System

* all or none of the requests will be executed 
* ACID
* transaction processing monitor

#### 1.3.3 Distributed Pervasive System

* nodes are small, mobile, and often embedded in a larger system
* Ubiquitous computing systems
* mobile computing systems
* Sensor networks
