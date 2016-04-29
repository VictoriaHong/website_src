---
title: Algorithms Part I - Week 0 & 1
date: 2016-04-21 17:45:34
tags: [Notes, Algorithms, Union Find, Memory]
categories: Tech Notes
layout: true
---
## Union-Find & Memory Usage for Algorithm

### Overview
|algorithm|initialize|union|find|description|
|:----------:|:-----------:|:-----------:|:---------:|:---------:|
|quick-find|N|N|1|union时loop更新有共同祖先的顶点|
|quick-union|N|N|N|union时只需要更新一个顶点的祖先|
|weighted QU|N|lg N|lg N|加一个size记录，小树总是连在大树上|


M union-find operations on a set of N objects:

|algorithm|worst-case time|
|:----------:|:-----------:|
|quick-find|MN|
|quick-union|MN|
|weighted QU|N + M log N|
|QU + path compression|N + M log N|
|Weighted QU + path compression|N + M log* N|

<!--more-->

### Dynamic Connectivity

- **Connected components**

- **Quick find / eager approach**

1. data structure

	int[] id = new int[]
	p and q are connected iff they have the same id

2. find: check is p and q have the same id

3. union: merger components containing p and q

	(id[i] == id[p] --> id[q])

```
public class QF
{
	private int[] id;

	public QF(int N)
	{
		id = new int[N];
		for (int i = 0; i < N; i++)
		{
			id[i] = i; // initialize, cost: N
		}
	}

	public boolean connected(int p, int q)
	{
		return id[p] == id[q]; // cost: 1
	}

	public void union(int p, int q) // too expensive, cost: N
	{
		int pid = id[p];
		int qid = id[q];
		for (int i = 0; i < id.length; i++)
		{
			if (id[i] == pid) id[i] = qid;
		}
	}
}
```
**quick-find is too slow for huge problem !**
**too flat so union is expensive !**

- **Quick union / lazy approach**

1. data structure

	int[] id = new int[]
	id[i] stores the parent of i
	root of i is id[id[id[...id[i]...]]]

2. find: check if p and q have the same root --> in the same union

3. union: set the id of p's root to the id of q's root

```
public class QU
{
	private int[] id;

	public QU(int N)
	{
		id = new int[N];
		for (int i = 0; i < N; i++)
		{
			id[i] = i; // initialize, cost: N
		}
	}

	public int root(int i)
	{
		while (i != id[i])
		{
			i = id[i]; // cost: depth of i
		}
		return i;
	}

	public boolean connected(int p, int q)
	{
		return root(p) == root(q); // cost: depth of p + depth of q
	}

	public void union(int p, int q)
	{
		int i = root(p); // cost: depth of p
		int j = root(q); // cost: depth of q
		id[i] = j;
	}
}
```
**too tall so find is expensive !**

- **Improve Quick-Union**

1. weighting to avoid tall trees : put smaller tree lower

    add a size array

    ```
    public void union(int p, int q)
	{
		int i = root(p); // cost: depth of p
		int j = root(q); // cost: depth of q
		if (i == j) return;
		if (size[i] < size[j])
		{
			id[i] = j;
			size[j] += size[i];
		}
		else
		{
			id[j] = i;
			size[i] += size[j];
		}
	}
	```

	union takes constant time, given roots. (log N)

	depth of any node is at most log N.

	find cost log N.

2. path compression

Two - pass: After computing root of p, set id of each examined node to point to that root.

One - pass: Make every other node in the path pointed to its **grandparent**.

```
public int root(int i)
	{
		while (i != id[i])
		{
			id[i] = id[id[i]];
			i = id[i]; // cost: depth of i
		}
		return i;
	}
```

### Memory Usage

|type|bytes|
|:--:|:--:|
|boolean|1|
|byte|1|
|char|2|
|int|4|
|float|4|
|long|8|
|double|8|


|type|bytes|
|:--:|:--:|
|chat[]|2N + 24|
|int[]|4N + 24|
|double|8N + 24|


|type|bytes|
|:--:|:--:|
|Object Overhead|16|
|Reference|8|
|padding|8x|
