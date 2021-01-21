# 17.3 Graphs

Trees are great, aren't they? But as we saw, we could draw some things using nodes and edges that weren't trees. Specifically, our restriction that there can only be one path between any two nodes didn't fit every situation. Let's see what happens when we get rid of that restriction.

{% embed url="https://www.youtube.com/watch?v=anRVomtGXFc" caption="" %}

## What is a graph?

A graph consists of:

* A set of nodes \(or vertices\)
* A set of zero of more edges, each of which connects two nodes. 

That's it! No other restrictions.

All of the structures below in green? Everything is a valid graph! The second one is also a tree, but none of the others are.

![](../.gitbook/assets/Screen%20Shot%202019-04-01%20at%201.18.57%20PM.png)

In general, note that **all trees are also graphs, but not all graphs are trees.**

## Simple Graphs only

Graphs can be divided into two categories: _simple_ graphs and _multigraphs_ \(or complicated graphs, a term I invented, because that's how I like to think of them.\) Fortunately, in this course \(and almost all applications and research\) focuses only on simple graphs. So when we say "graph" in this course, you should always think of a "simple graph" \(unless we say otherwise.\)

Well, it's time to address the elephant in the room. What's a simple graph?

![](../.gitbook/assets/Screen%20Shot%202019-03-17%20at%204.19.24%20PM.png)

Look at the graphs in red. The graph in the middle has 2 distinct edges going from/to bottom-next to/from bottom-left node. In other words, there are multiple edges between two nodes. This is **not** a simple graph, and we ignore their existence unless specified otherwise. Graphs like these are called multigraphs.

Look at the third graph. It has a loop! An edge from a node to itself! We don't allow this either. Graphs like these are sometimes categorized as multigraphs, and sometimes, even multigraphs explicitly ban self-loops.

## More categorizations.

Graphs are simple in the following text, and in this course, unless specified otherwise. But there are more categorizations.

There are undirected graphs, where an edge `(u, v)` can mean that the edge goes from the nodes u to v and from the nodes v to u too. There are directed graphs, where the edge `(u, v)` means that the edge starts at u, and goes to v \(and the vice versa is not true, unless the edge `(v, u)` also exists.\) Edges in directed graphs have an arrow.

There are also acyclic graphs. These are graphs that don't have any cycles. And then there are cyclic graphs, i.e., there exists a way to start at a node, follow some **unique** edges, and return back to the same node you started from.

![](../.gitbook/assets/Screen%20Shot%202019-03-17%20at%204.24.36%20PM.png)

In the above picture, we can clearly see the difference between how we draw directed and undirected edges.

Take a look at the cyclic graphs. If you start at `a`, you can run back around using only distinct edges and get back to `a`. Thus, the graph is cyclic.

Take a look at the top-left graph. Is there any node `n`, such that if you start at `n`, you can follow some distinct edges, and get back to `n`? Nope! \(Remember than for directed edges, you must follow the directions. You can go from `a` to `b` but not `b` to `a`.\)

## More definitions

![](../.gitbook/assets/Screen%20Shot%202019-03-17%20at%204.26.41%20PM.png)

