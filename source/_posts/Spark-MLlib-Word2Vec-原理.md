---
title: Spark MLlib Word2Vec 原理
date: 2018-06-12 19:01:00
tags:
    - Spark
    - Word2Vec
    - ML
    - NLP
    - 小记
---

基于Spark的word2vec采用的是基于Hierarchical Softmax的Skip-gram模型

### Skip-gram 模型
<img src='/picture/mllib-word2vec-skipgram.png'  style='zoom:50%'/>
结构：输入层+隐藏层+输出层
输入：某个词的词向量
输出：该词对应的上下文，若上下文的窗口为5，那么输出就是softmax概率排名前10的词。

<!--more-->

**缺点：**我们往往训练的语料很大，所以词汇表就会很大，输出层softmax就需要计算所有词的概率，计算量非常大，为改进这个问题提出了分层softmax。

### 基于Hierarchical Softmax的Skip-gram

#### Hierarchical Softmax
去掉了隐层，输出层变为哈夫曼树，如下所示
<img src='/picture/mllib-word2vec-model.png' style= 'zoom:70%' />
分层softmax采用了二分类的思想，按照词频构造一棵哈夫曼树， 单词表中的单词均为二叉树的叶子结点。某个叶子结点的概率为，从二叉树的根节点到该叶子节点的路径上各个节点与输入单词的向量点乘的连乘。

#### Model
![](/picture/mllib-word2vec.png)


