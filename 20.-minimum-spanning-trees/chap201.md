# 20.1 MSTs and Cut Property

{% embed url="https://youtu.be/vnKK38JS9Ik" caption="" %}

{% embed url="https://youtu.be/VwwWsr4MLME" caption="" %}

{% embed url="https://youtu.be/r\_4Ei251fDU" caption="" %}

{% embed url="https://youtu.be/50K-QvOHfOE" caption="" %}

A minimum spanning tree \(MST\) is the lightest set of edges in a graph possible such that all the vertices are connected. Because it is a tree, it must be connected and acyclic. And it is called "spanning" since all vertices are included.

In this chapter, we will look at two algorithms that will help us find a MST from a graph.

Before we do that, let's introduce ourselves to the Cut Property, which is a tool that is useful for finding MSTs.

## Cut Property

{% embed url="https://youtu.be/QYdZS4S-FyU" caption="" %}

We can define a **cut** as an assignment of a graph’s nodes to two non-empty sets \(i.e. we assign every node to either set number one or set number two\).

We can define a **crossing edge** as an edge which connects a node from one set to a node from the other set.

With these two definitions, we can understand the **Cut Property**; given any cut, the minimum weight crossing edge is in the MST.

![](../.gitbook/assets/Screen%20Shot%202019-04-14%20at%208.57.22%20PM.png)

The proof for the cut property is as follows: Suppose \(for the sake of contradiction\) that the minimum crossing edge _e_ were not in the MST. Since it is not a part of the MST, if we add that edge, a cycle will be created. Because there is a cycle, this implies that some other edge f must also be a crossing edge \(for a cycle, if _e_ crosses from one set to another, there must be another edge that crosses back over to the first set\). Thus, we can remove _f_ and keep _e_, and this will give us a lower weight spanning tree. But this is a contradiction because we supposedly started with a MST, but now we have a collection of edges which is a spanning tree but that weighs less, thus the original MST was not actually minimal. As a result, the cut property must hold.

Here is a diagram illustrating some of the arguments of the above proof: ![](../.gitbook/assets/Screen%20Shot%202019-04-14%20at%209.03.06%20PM.png)

