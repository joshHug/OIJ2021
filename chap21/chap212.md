# 21.2 Shortest Path on DAGs

Recall from the previous section that **DAGs** are **directed, acyclic graphs**. If we wanted to find the shortest path on DAGs we could use [Dijkstra's](https://github.com/joshhug/hug61b/tree/e1d84817521747a76f17d2ed077abab493505c3f/chap192.md). However, with DAGs there's a simple shortest path algorithm which also handles negative edge weights!

## Dijkstra's Negative Edge Weight Failure

Recall that Dijkstra's can fail if negative edges exist because it relies on the assumption that once we visit an edge, we've found the shortest path to that edge. But if negative edge weights can exist ahead of where we can see, then this assumption fails. Consider the following example:

![](../.gitbook/assets/21.2.1.png)

Starting from A, Dijkstra's will visit C first, then B \(never even considering the edge $$B \rightarrow C$$\).

Of course, negative edge weights do not mean Dijkstra's is guaranteed to fail. Dijkstra's succeeds with the following example:

![](../.gitbook/assets/21.2.2.png)

## Shortest Path Algorithm for DAGs

Visit vertices in topological order:

* On each visit, relax all outgoing edges

Recall the definition for relaxing an edge $$u \rightarrow v$$ with weight $$w$$:

```text
if distTo[u] + w < distTo[v]:
    distTo[v] = distTo[u] + w
    edgeTo[v] = u
```

Since we visit vertices in topological order, a vertex is visited only when all possible info about it has been considered. This means that if negative edge weights exist along a path to $$v$$, then those have been taken into account by the time we get to $$v$$!

Finding a topological sort takes $$O(V + E)$$ time while relaxation from each vertex also takes $$O(V + E)$$ time in total. Thus, the overall runtime is $$O(V + E)$$. Recall that Dijkstra's takes $$O((V + E)\log V)$$ time because of our min-heap operations.

What if we want to solve the shortest path problem on graphs that aren't DAGs and also may have negative edges? An extension of Dijkstra's called [Bellman Ford](https://en.wikipedia.org/wiki/Bellman–Ford_algorithm) can suit your needs, though it is out of scope for this course.

