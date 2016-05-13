---
title: Bittiger 硅谷之路 9
date: 2016-05-12 15:40:36
tags: [Notes, Bittiger, 硅谷之路, Structure]
categories:
- Tech Notes
---

## 深入浅出 Google File System

### Overview

Google 三篇论文：Google File System, BigTable, MapReduce.

Structure:

![](http://i.imgur.com/aEE7aX1.png)

<!--more-->

### Store a File

Disk 有一部分存文件的 metadata，包括文件属性（name, createdTime, size）以及索引。

Block 存文件具体内容 offset。一个 block 大小为 1024 bytes。

### Store a Big File

用 chunk 来存。一个 chunk 大小为 64 MB。等于 64K 个 block。

Advantages: less metadata; less data flow.

Disadvantages: small files waste space.

### Store supper big file

索引放在 master 设备上，记录 chunk server 和 offset。

Disadvantages: chunk server 的变化要通知 master。这样加重了 master 负担。

Solution: master 不记录每块数据的 offset，只记录在哪个 chunk server 上。低耦合。

### Find damaged data

每个 block 存一个 32 bit 的 checksum。读取时验证一下。

### Duplicate Chunk Server

- 3 replicates
- 跨机架、跨中心：数据中心建立在不同地方，每个地方又有放在不同机架上的副本。

### Recover

Get help from master to get right data when find damaged data.

Master monitors all slaves' heart-beat to recognize dead one.

Recover strategy based on the number of alive duplicates. 副本少的先修复。

### Deal with hot spot

Duplicate chunk server based on disk and bandwidth utilization.

### How to read a file

- Read:

  ![](http://i.imgur.com/oJEUmF6.png)

  key points:

    - chunk index 是可以由 client 自己计算出来的，大小固定的。
    - 找到最近的 chunk server。

- Write:

  ![](http://i.imgur.com/ZtpT1gn.png)

  key points:

  - 主 chunk server 是随机选的，只对这一次的操作负责。
  - 要写的东西是发给最近的 chunk server 而不是主 server。
  - chunk server 之间传输速度很快。分级传递即可，不需要并发。
  - 主 server 跟 client 通讯，告知全部 cache 好了，再通知各个从属 server 一起写。先缓存再写入可以消除大部分写失败。
  - 发生写错误的时候，交给上级的 GFS master 去处理。下级的功能越单一越好。
