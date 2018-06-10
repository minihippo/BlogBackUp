---
title: markdown语法
date: 2017-09-26 19:20:57
tags:
     - 备份
     - markdwon
     - 教程
toc: true
reward: true
---

## Overview
<img src="/picture/markdown语法.png", style="zoom:50%">

<!--more-->

## Headers
```shell
# This is an H1  
## This is an H2  
### This is an H3
#### This is an H4
##### This is an H5
###### This is an H6 
```
# This is an H1
## This is an H2
### This is an H3
#### This is an H4
##### This is an H5
###### This is an H6

test over...

## Lists
#### Unordered  
```shell
* 1
* 2
- 1
- 2
+ 1
+ 2
```
the normal size 1
* 1
  the normal size
* 2 
- 1
- 2
+ 1
+ 2 

#### Ordered
```shell
1. 1
   * unordered sub-list
2. 2
   1. ordered sublist
3. 3
   You can have properly indented paragraphs within list items. Notice the blank line above, and the leading spaces (at least one, but we'll use three here to also align the raw Markdown).

   To have a line break without a paragraph, you will need to use two trailing spaces.⋅⋅
   Note that this line is separate, but within the same paragraph.⋅⋅
   (This is contrary to the typical GFM line break behaviour, where trailing spaces are not required.)
```

the normal size
1. 1
  the normal size
  - unordered sub-list
  - iiiii
2. 2
   1. ordered sublist
3. 3
   You can have properly indented paragraphs within list items. Notice the blank line above, and the leading spaces (at least one, but we'll use three here to also align the raw Markdown).

   To have a line break without a paragraph, you will need to use two trailing spaces.⋅⋅
   Note that this line is separate, but within the same paragraph.⋅⋅
   (This is contrary to the typical GFM line break behaviour, where trailing spaces are not required.)

#### Assignment
```shell
- [ ] eat
- [x] sleep
```
- [ ] eat
   - [ ] test1
   - [ ] test2
- [x] sleep

## Blockquotes
#### Contents
```shell
> For example
```
> For example  

#### Page Links
```shell
[baidu](http://www.baidu.com)
```
[baidu](http://www.baidu.com)

#### Picture Links
```shell
![title](path)
```

## Code Blocks
#### Single Line
```shell
`code`
```
`code`

#### Block
```java
```java
//test
int a = 5
``\`
```
```java
//test
int a = 5
```

## Font
#### Blod
```shell
**blod**
```
**blod**

#### Italic
```shell
*italic*
```
*italic*

#### Underline
```shell
<u>underline</u>
```
<u>underline</u>

#### Horizontal Rules
```shell
*****
____
```
***************

#### Line-through
```shell
~~line-through~~
```
~~line-through~~

#### Backslash Escapes
```shell
\*bacslash escape\*
```
\*bacslash escape\*


## Tables
```shell
| name1 | name2 | name3 |
| ----- | ----- | ----- |
| 1     | 2     | 3     |
| 1     | 2     | 3     |
```
| name1 | name2 | name3 |
| ----- | ----- | ----- |
| 1     | 2     | 3     |
| 1     | 2     | 3     |
  
## Peference
#### Mathematical Expression
```shell
$ lim_{x \to \infty}\exp(-x) = 0$
```
$ lim_{x \to \infty}\exp(-x) = 0$

#### Superscript
```shell
y^2^ = 4
```
y^2^ = 4

#### Subscript
```shell
H~2~O
```
H~2~O 



