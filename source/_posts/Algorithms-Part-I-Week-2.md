---
title: Algorithms Part I Week 2
date: 2016-05-06 23:35:09
tags: [Notes, Algorithms]
categories: Tech Notes
---

## Stacks and Queues & Elementary Sorts

### Overview

- Modular programming style
- For stack, push pop
	- Each operation cost constant time in the worst case
	- Extra time and space to deal with the link
- For queue, enqueue dequeue
	- Each operation cost constant amortized time
	- less wasted space
- Generics
- Iterator

<!--more-->

### Stacks

**Linked-list implementation**

```
public class LinkedStackOfStrings {
	private Node first = null;

	private class Node {
		String item;
		Node next;
	}

	public boolean isEmpty(){
		return first == null;
	}

	public String pop(){
		String item = first.item;
		first = first.next;
		return item;
	}

	public void push(String item){
		Node oldFirst = first;
		first = new Node();
		first.item = item;
		first.next = oldFirst;
	}
}

```

Performance:

- Operations takes constant time
- 40N bytes

**Array implementation**

s[N]

push() --> add new item at s[N]

pop() --> remove item from s[N-1]

Defects:

- May cause stack overflow when N exceeds capacity
- Loitering problem (after pop, still keep a reference, can't be GCed)

**Resizing-array implementation**

push() --> double size of s[] when it is full

pop() --> halve size of s[] when it is one-quarter full

Performance:

- operation

| | best | worst | aver |
|:-:|:-:|:-:|:-:|
| construct | 1 | 1 | 1 |
| push | 1 | N | 1 |
| pop | 1 | N| 1 |
| size | 1 | 1 | 1 |

- 8N when full; 32N when one-quarter full

### Queue

**Linked-list implementation**

```
public class LinkedQueueOfStrings{
	private Node first, last;

	private class Node {
		String item;
		Node next;
	}

	public boolean isEmpty(){
		return first == null;
	}

	public void enqueue(String item){
		Node oldLast = last;
		last = new Node();
		last.item = item;
		last.next = null;
		if (isEmpty()) first = last;
		else oldLast.next = last;
	}

	public String dequeue(){
		String item = first.item;
		first = first.next;
		if (isEmpty()) last = null;
		return item;
	}
}
```

### Generics

In a nutshell, generics enable types (classes and interfaces) to be parameters when defining classes, interfaces and methods.


```
public class FixedCapacityStack(int capacity){
	s = (Item[]) new Object[capacity];
}
```

Java don't allow create an array in generic type, so an ugly cast is needed here. Get a warning.

Primitive type has a wrapper class. Autoboxing can cast btw a primitive type and its wrapper automatically behind the scenes.

### Iterators

Iterable is an interface which can return an iterator.

Iterator is an interface which has methods: hasNext() and next().

Foreach statement is equivalent to

```
Iterator<String> i = stack.iterator();

while(i.hasNext()){
	String s = i.next();
	//do sth with s
}
```
When order doesn't matter, we use **Bag API**. Bag implements iterable interface. Method: add(), size(), iterator().

### Stack and queue applications

List collection implements iterable interface. But it is too broad.

Dijkstra's **[two-stack algorithm](http://algs4.cs.princeton.edu/13stacks/Evaluate.java.html)** to evaluate n arithmetic expression. Value stack and Operator stack. Omit '(' but pop when ')'.

### Sorting introduction

Basic idea: callback = reference to executable code

Method: implements Comparable interface. Write compareTo method.
