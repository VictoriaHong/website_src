---
title: Bittiger 硅谷之路 44 & 45
date: 2016-04-29 15:22:17
tags: [Notes, Bittiger, 硅谷之路, Crawler, concurrency]
categories:
- Tech Notes
---

## Overview

How to write crawler code and how to improve it step by step.

Multi-thread concurrency design with mutex and semaphore.

<!--more-->

### Crawler

```
import urllib2
url='http://yue.ifeng.com/a/20150717/39747647_0.shtml'
request = urllib2.Request(url)
response = urllib2.urlopen(request)
page = response.read()
webFile = open('webPage.html','wb')
webFile.write(page)
webFile.close()
```

### Q1: What is the network process after crawl a web page?

|Step|Crawler|Web Server|
|:-:|:-----:|:--------:|
|0|socket()|socket()|
|||bind()|
|||listen()|
|1|connect()|accept()|
|2|read() & write()|read() & write()|
|3|close()|close()|

TCP 3-way handshake: SYN, SYN-ACK, ACK

- Layers

	Application layer `FTP HTTP DNS`

	Abstract layer `Socket`

	Transportation layer `TCP UDP`

	Network layer `IP`

	Link layer `Hardware interface`

	[Reference](http://coolshell.cn/articles/11564.html)

### Q2: What is HTML

Tree structure

### Q3: crawl all the news from a website

- identify a list page
- identify the links with and find a pattern
- find all with regex

**Architecture v1**

list crawler --> links of news --> news crawler --> pages of news

### Q4: crawl form other websites

**Architecture v2**

design a crawler for each website(list and page)

**Architecture v3**

same crawler for all websites

scheduler has a **taskTable**: id, priority(0,1), type(list, page), state(new, done, working), link, availableTime, endTime

```
while(true)
  lock(taskTable)
  while taskTable.find("state=new") == null
    Release(taskTable)
    Sleep(1s)
    lock(taskTable)

  task = taskTable.findOne("state=new")
  task.state = "working"
  Release(taskTable)

  page = Crawl(task.url)

  if task.type == "list"
    lock(taskTable)
    For newTask in page:
      taskTable.Add(newTask)
    task.state = "done"
    Release(taskTable)
  Else
    lock(pageTable)
    pageTable.Add(page)
    Release(pageTable)
    lock(taskTable)
    task.state = "done"
    Release(taskTable)
```

### Q5: design scheduler with conditional variable

```
while(true)
  lock(taskTable)
  while taskTable.find("state=new") == null
    Cond_Wait(cond, taskTable)
  task = taskTable.findOne("state=new")
  task.state = "working"
  Release(taskTable)

  page = Crawl(task.url)

  if task.type == "list"
    lock(taskTable)
    For newTask in page:
      taskTable.Add(newTask)
      Cond_Signal(cond)
    task.state = "done"
    Release(taskTable)
  Else
    lock(pageTable)
    pageTable.Add(page)
    Release(pageTable)
    lock(taskTable)
    task.state = "done"
    Release(taskTable)
```
```
Cond_Wait(cond, mutex)
  Lock(cond.threadWaitList)
  cond.threadWaitList.Add(this.thread)
  Release(cond.threadWaitList)

  Release(mutex)
  Block(this.thread)
  Lock(mutex)


Cond_Signal(cond)
  Lock(cond.threadWaitList)
  if cond.threadWaitList.size > 0
    thread = cond.threadWaitList.Pop()
    Wakeup(thread)
   Release(cond.threadWaitList)
```

### Q6: design scheduler with semaphore

```
while(true)
  Wait(numberOfNewTask)
  Lock(taskTable)
  task = taskTable.findOne("state=new")
  task.state = "working"
  Release(taskTable)

  page = Crawl(task.url)

  if task.type == "list"
    lock(taskTable)
    For newTask in page:
      taskTable.Add(newTask)
      Signal(numberOfNewTask)
    task.state = "done"
    Release(taskTable)
  Else
    lock(pageTable)
    pageTable.Add(page)
    Release(pageTable)
    lock(taskTable)
    task.state = "done"
    Release(taskTable)
```

```
Wait(semaphore)
  Lock(semaphore)
  semaphore.val--
  if semaphore.val < 0
    semaphore.processWaitList.Add(this.process)
    Release(semaphore)
    Block(this.process)
  Else
    Release(semaphore)

Signal(semaphore)
  Lock(semaphore)
  semaphore.val++
  if semaphore.val <= 0
    process = semaphore.processWaitList.Pop()
    Wakeup(process)
  Release(semaphore)
```

### Q7: Design fastest consumer and producer

Fill in later.
