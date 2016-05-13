---
title: Bittiger 硅谷之路 7
date: 2016-05-11 22:02:12
tags: [Notes, Bittiger, 硅谷之路, Structure]
categories:
- Tech Notes
---

## Lessons Learned in Scaling Twitter

### 一周结构及缺陷

  Monarail + MySQL

  Defects:
    - MySQL难扩展：表格关系复杂；分布式更增加join复杂型
    - 小变化也要全部部署，deploy时间长
    - 性能差
    - 架构混乱

<!--more-->

### 改进

  1. 存储分开

      - Tweets(gizzard)
      - Users(gizzard)
      - Timelines(redis)
      - SocialGraph(gizzard)

  2. 路由、展式、逻辑分开

      - TFE
      - Web, API, Monorail
      - Tweets, Users, Timelines

      ![](http://i.imgur.com/gQqOPnZ.png)

      ![](http://i.imgur.com/RSsaj7d.png)

### 解决方案

  - 构建：开源的Twitter Server：配置、管理、日志、生命周期、监控服务
  - 服务通信：开源的Finagle：服务发现、负载均衡、重试、连接池、统计数据收集、分布调试
  - 调用服务：做什么（service as a function）& 怎么做（底层优化）
  - 找到最优策略：注意观察median，而不是平均值
  - 监控：zipkin
