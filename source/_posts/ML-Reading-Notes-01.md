---
title: ML Reading Notes-01
date: 2018-01-15 21:43:44
tags:
    - ML
    - notes
toc: true
reward: true
---
# Introduction

**machine learning**: look for a function to do something like speech recognition etc.

<!--more-->

<img src="/ML-ReadingNotes/1-1.png" style="zoom:40%" />

# Learning Map

overview

<img src="/ML-ReadingNotes/1-2.png" style="zoom:40%">

| no.  | scenario                 | task/diff                     | difference                          |
| ---- | ------------------------ | ----------------------------- | ----------------------------------- |
| 1    | Supervised Learning      |                               | labelled                            |
| 2    |                          | Regression                    | $f$ output: scalar                  |
| 3    |                          | Classification                | $f$ output: classification          |
| 4    |                          | Structured Learning           | $f$ output: structure               |
| 5    | Semi-Supervised Learning | data is related to task       | labelled & unlabelled               |
| 6    | Transfer Learning        | data is unrelated             | labelled & unlabelled               |
| 7    | Unsupervised Learning    | Like word2vec                 | machine learns meaning              |
| 8    | Reinforcement Learning   | have no certain or unique ans | learn from critics, better or worse |

### Supervised Learning

##### 1. Regression

the output of target function $f$ is scalar.

##### 2. classification

binary classification: the output of target function $f$ is yes/no.   
Multi-class classification: the ouput of target function $f$ is classification

1&2 method: linear model and Non-linear Model, deep learning is one of non-linear model for complicated problems.

##### 3. Structured Learning

the output of target function $f$ is structured data.  
比如，根据一段语音得到对应文本。

#### Semi-Supervised Learning & Transfer Learning 

both labelled and unlabelled data, semi-supervised data is related to the task considered.   
For example, recognizing cat and dogs, transfer learning:  
<img src="/ML-ReadingNotes/1-3.png" style="zoom:40%">


