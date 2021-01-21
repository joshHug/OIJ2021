# 18.1 BFS

In [Chapter 17.4](https://joshhug.gitbooks.io/hug61b/content/chap17/chap174.html), we developed DFS \(Depth First Search\) Traversal for graphs. In DFS, we visit down the entire lineage of our first child before we even begin to look at our second child - we literally search _**depth first**_.

Here, we will talk about BFS \(Breadth First Search\) \(also known as Level Order Traversal\). In BFS, we visit all of our immediate children before continuing on to any of our grandchildren. In other words, we visit all nodes 1 edges from our source. Then, all nodes 2 edges from our source, etc.

The pseudocode for BFS is as follows:

```text
Initialize the fringe (a queue with the starting vertex) and mark that vertex.
Repeat until fringe is empty:
Remove vertex v from the fringe.
For each unmarked neighbor n of v:
Mark n.
Add n to fringe.
Set edgeTo[n] = v.
Set distTo[n] = distTo[v] + 1.
```

A _fringe_ is just a term we use for the data structure we are using to store the nodes on the frontier of our traversal's discovery process \(the next nodes it is waiting to look at\). For BFS, we use a queue for our fringe.

`edgeTo[...]` is a map that helps us track how we got to node `n`; we got to it by following the edge from `v` to to `n`.

`distTo[...]` is a map that helps us track how far `n` is from the starting vertex. Assuming that each edge is worth a distance of `1`, then the distance to `n` is just one more than the distance to get to `v`. Why? We can use the way we know how to get to `v`, then pay one more to arrive at `n` via the edge that necessarily exists between `v` and `n` \(it must exist since in the `for` loop header, `n` is defined as a neighbor of `v`\).

This [slide deck](https://docs.google.com/presentation/d/1JoYCelH4YE6IkSMq_LfTJMzJ00WxDj7rEa49gYmAtc4/edit#slide=id.g76e0dad85_2_380) illustrates how this pseudocode can be carried out on an example graph.

## DFS vs BFS

**Question 18.1**: What graph traversal algorithm uses a stack rather than a queue for its fringe?

**Answer 18.1**: DFS traversal.

Note however that DFS and BFS differ in more than just their fringe data structure. They differ in the order of marking nodes. For DFS we mark nodes only once we visit a node - aka pop it from the fringe. As a result, it's possible to have multiple instances of the same node on the stack at a time if that node has been queued but not visited yet. With BFS we mark nodes as soon as we add them to the fringe so this is not possible.

Recursive DFS implements this naturally via the recursive stack frames; iterative DFS implements it manually:

```text
Initialize the fringe, an empty stack
    push the starting vertex on the fringe
    while fringe is not empty:
        pop a vertex off the fringe
        if vertex is not marked:
            mark the vertex
            visit vertex
            for each neighbor of vertex:
                if neighbor not marked:
                    push neighbor to fringe
```

