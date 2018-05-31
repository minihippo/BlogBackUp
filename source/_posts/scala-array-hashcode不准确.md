---
title: scala array hashcode不准确
date: 2018-03-21 21:01:47
tags:
    - scala
    - 小记
---

　　因为scala的array就是在java array的基础上的，所以hashcode直接用的就是java定义的hashcode。而java的`array.hashcode()`就是根据数组的引用地址计算的，而不是根据数据的内容。
 
**解决方案**：
1. 转成asjava,用`java.util.Arrays.hashcode()`
2. 转成Seq或者list
3. 重写hashcode，但是数组计算hashcode效率会变差
