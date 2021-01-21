# 18.2 Representing Graphs

Let's spend some time now talking about how to implement these graphs and graph algorithms in code.

We will discuss our choice of **API**, and also the **underlying data structures** used to represent the graph. Our decisions can have profound implications on our _runtime_, _memory usage_, and _difficulty of implementing various graph algorithms_.

## Graph API

An API \(Application Programming Interface\) is a list of methods available to a user of our class, including the method signatures \(what arguments/parameters each function accepts\) and information regarding their behaviors. You have already seen APIs from the Java developers for the classes they provide, such as the [Deque](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Deque.html).

For our Graph API, let's use the common convention of assigning each unique node to an integer number. This can be done by maintaining a map which can tell us the integer assigned to each original node label. Doing so allows us to define our API to work with integers specifically, rather than introducing the need for generic types.

We can then define our API to look something like this perhaps:

```text
public class Graph {
  public Graph(int V):               // Create empty graph with v vertices
  public void addEdge(int v, int w): // add an edge v-w
  Iterable<Integer> adj(int v):      // vertices adjacent to v
  int V():                           // number of vertices
  int E():                           // number of edges
...
```

Clients \(people who wish to use our Graph data structure\), can then use any of the functions we provide to implement their own algorithms. The methods we provide can have a significant impact on how easy/difficult it may be for our clients to implement particular algorithms.

## Graph Representations

Next, we'll talk about the underlying data structures that can be used to represent our graph.

### Adjacency Matrix

One way we can do this is by using a 2D array. There is an edge connecting vertex `s` to `t` iff that corresponding cell is `1` \(which represents `true`\). Notice that if the graph is undirected, the adjacency matrix will be symmetric across its diagonal \(from the top left to the bottom right corners\). ![](../.gitbook/assets/Screen%20Shot%202019-03-27%20at%201.58.11%20AM.png) ![](../.gitbook/assets/Screen%20Shot%202019-03-27%20at%202.03.44%20AM.png)

### Edge Sets

Another way is to store a single set of all the edges.

![](../.gitbook/assets/Screen%20Shot%202019-03-27%20at%202.03.36%20AM.png) ![](../.gitbook/assets/Screen%20Shot%202019-03-27%20at%202.03.44%20AM%20%282%29.png)

### Adjacency Lists

A third way is to maintain an array of lists, indexed by vertex number. Iff there is an edge from `s` to `t`, the list at array index `s` will contain `t`.

![](../.gitbook/assets/Screen%20Shot%202019-03-27%20at%202.05.55%20AM.png) ![](../.gitbook/assets/Screen%20Shot%202019-03-27%20at%202.03.44%20AM%20%281%29.png)

In practice, adjacency lists are most common since graphs tend to be sparse \(there are not many edges in each bucket\).

### Efficiency

Your choice of underlying data structure can impact the runtime and memory usage of your graph. This table from the [slides](https://docs.google.com/presentation/d/143WntPl7CG5Po3utVK0jYSA0Jd6XKppT5h1juWEWhUU/edit#slide=id.g54593997ea_0_422) summarizes the efficiencies of each representation for various operations. Do not copy this on to your cheatsheet without taking the time to first understand where these bounds come from. The lecture contained walkthroughs explaining the rationale behind several of these cells.

![](../.gitbook/assets/Screen%20Shot%202019-03-27%20at%202.09.01%20AM.png)

Further, DFS/BFS on a graph backed by adjacency lists runs in $$O(V+E)$$, while on a graph backed by an adjacency matrix runs in $$O(V^2)$$. See the [slides](https://docs.google.com/presentation/d/143WntPl7CG5Po3utVK0jYSA0Jd6XKppT5h1juWEWhUU/edit#slide=id.g128520644b_0_91) for help in understanding why.

