---
title: Bittiger 硅谷之路 10
date: 2016-05-12 21:45:38
tags: [Notes, Bittiger, 硅谷之路, Structure]
categories:
- Tech Notes
---

## 深入浅出BigTable

### Overview

  NoSQL db

  使用<k, v>的物理结构存储超大表

  BigTable吃内存，GFS吃硬盘

<!--more-->

### 文件内快速查询

Table = a list of sorted <key, value> pairs

### 保存很大的表

A big table = a list of tablets

A tablet = a list of sorted <key, value> pairs

表内有序，表间无序

### 保存超大表

再拆出来一层小小表

### 如何写数据

不直接写入硬盘

写入 memTable (内存表)来加速: <key, value> pairs

A tablet = memTable + a list of smallTables

当内存表满了（64MB），写入硬盘。

### 避免数据丢失

内存可能丢失

写入 memTable 的同时，写入硬盘内 tableLog。

A tablet = memTable + a list of smallTables + log

### 如何读数据

写策略造成：需要查找所有的小小表和memTable, 很慢

加速：+ index + bloomfilter
