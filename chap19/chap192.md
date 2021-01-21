# 19.2 Dijkstra's

## Do it by hand

![](../.gitbook/assets/Screen%20Shot%202019-03-23%20at%207.27.24%20PM.png)

Do the following two things.

1. Find a path from the vertex labeled $$0$$ to the vertex labeled $$5$$. 
2. Find a shortest-paths tree from the vertex labeled $$0$$. \(i.e., find the shortest path from $$0$$ to every single vertex in the graph.\)
3. Try to come up with an algorithm to do this. 

\(The solutions to 1 and 2 are at the end of this section.\)

## Observations

Note that the shortest path \(for a graph whose edges have weights\) can have many, many edges. What we care to minimize is the sum of the weights of the edges on the selected path.

Secondly, note the fact that the shortest paths tree from a source $$s$$ can be created in the following way:

* For every vertex $$v$$ \(which is not $$s$$\) in the graph, find the shortest path from $$s$$ to $$v$$. 
* "Combine"/"Union" all the edges that you found above. Tada!

Thirdly, note that the "Shortest Path Tree" will **always be a tree**. Why? Well, let's think about our original solution, where we maintained an $$\texttt{edgeTo}$$ array. For every node, there was exactly one "parent" in the $$\texttt{edgeTo}$$ array. \(Why does this imply that the "Shortest Path Tree" will be a tree? Hint: A tree has $$V-1$$ edges, where $$V$$ is the number of nodes in the tree.\)

## Dijkstra's Algorithm \[\[/ˈdaɪkstrə/\]\]

Dijkstra's algorithm takes in an input vertex $$s$$, and outputs the shortest path tree from $$s$$. How does it work?

1. Create a priority queue.
2. Add $$s$$ to the priority queue with priority $$0$$. Add all other vertices to the priority queue with priority $$\infty$$. 
3. While the priority queue is not empty: pop a vertex out of the priority queue, and **relax** all of the edges going out from the vertex.

### What does it mean to **relax**?

Suppose the vertex we just popped from the priority queue was $$v$$. We'll look at all of $$v$$'s edges. Say, we're looking at edge $$(v, w)$$ \(the edge that goes from $$v$$ to $$w$$\). We're going to try and relax this edge.

What that means is: Look at your current best distance to $$w$$ from the source, call it $$\texttt{curBestDistToW}$$. Now, look at your $$\texttt{curBestDistTo}\textbf{V} + \texttt{weight}(v, w)$$ \(let's call it $$\texttt{potentialDistToWUsingV}$$.

Is $$\texttt{potentialDistToWUsingV}$$ **better, i.e., smaller** than $$\texttt{curBestDistToW}$$? In that case, set $$\texttt{curBestDistToW} = \texttt{potentialDistToWUsingV}$$, and update the $$\texttt{edgeTo}[w]$$ to be $$v$$.

**Important note: we never relax edges that point to already visited vertices.**

This whole process of calculating the potential distance, checking if it's better, and potentially updating is called relaxing.

Alternate definition is captured by the following image. ![](../.gitbook/assets/pz1muo.jpg)

### Pseudocode

```text
def dijkstras(source):
    PQ.add(source, 0)
    For all other vertices, v, PQ.add(v, infinity)
    while PQ is not empty:
        p = PQ.removeSmallest()
        relax(all edges from p)
```

```text
def relax(edge p,q):
   if q is visited (i.e., q is not in PQ):
       return

   if distTo[p] + weight(edge) < distTo[q]:
       distTo[q] = distTo[p] + w
       edgeTo[q] = p
       PQ.changePriority(q, distTo[q])
```

### Guarantees

As long as the edges are all non-negative, Dijkstra's is guaranteed to be optimal.

### Proofs and Intuitions

Assume all edges are non-negative.

* At start, distTo\[source\] = 0. This is optimal.
* After relaxing all edges from source, let vertex $$v_1$$ be the vertex with the minimum weight \(i.e., the one that's closest to the source.\) **Claim: distTo\[**$$v_1$$**\] is optimal, i.e., whatever the value of distTo\[**$$v_1$$**\] is at this point is the shortest distance from** $$s$$ **to** $$v_1$$. Why?
  * Let's try to see why this **MUST** be the case. 
  * Suppose that it isn't the case. Then that means that there is some other path from $$s$$ to $$v_1$$ which is shorter than the direct path $$(s, v_1)$$. Ok, so let's consider this hypothetical cool shorter path... it would have to look like $$(s, v_a, v_b, \ldots, v_1)$$. But... $$(s, v_a)$$ is **already** bigger than $$(s, v_1)$$. \(Note that this is true because $$v_1$$ is the vertex that is closest to $$s$$ from above.\) So how can such a path exist which is actually shorter? It can't! 
* Now, the next vertex to be popped will be $$v_1$$. \(Why? Note that it currently has the lowest priority in the PQ!\)
* So now, we can make this same argument for $$v_1$$ and all the relaxation it does. \(This is called "proof by induction". It's kind of like recursion for proofs.\) And that's it; we're done.

### Negative Edges?

Things can go pretty badly when negative edges come into the picture. Consider the following image.

![](../.gitbook/assets/Screen%20Shot%202019-03-23%20at%208.10.37%20PM.png)

Suppose you're at that vertex labeled $$34$$. Now you're going to try to relax all your edges. You have only one outgoing edge from yourself to $$33$$ with weight $$-67$$. Ah, but note: vertex $$33$$ is already visited \(it's marked with white.\) So... we don't relax it. \(Recall the pseudocode for the relax method.\)

Now we go home thinking that the shortest distance to $$33$$ is $$82$$ \(marked in pink.\) But really, we should have taken the path **through** $$34$$ because that would have given us a distance of $$101 - 67 = 34$$. Oops.

**Dijkstra's algorithm is not guaranteed to be correct for negative edges. It might work... but it isn't guaranteed to work.**

Try this out: suppose that your graph has negative edges, but all the negative edges only go out of the source vertex $$s$$ that you were passed in. Does Dijkstra's work? Why / Why not?

### A noteworthy invariant

Observe that once a vertex is popped off the priority queue, it is never re-added. Its distance is never re-updated. So, in other words, once a vertex is popped from the priority queue, we **know** the true shortest distance to that vertex from the source.

One nice consequence of this fact is "short-circuiting". Suppose... that I didn't care about the shortest-paths tree, but just wanted to find the shortest path from some source to some other target. Suppose that you wanted to take, like, the cities of the world on a graph, and find the shortest path from Berkeley to Oakland. Running `dijkstra(Berkeley)` will mean that you can't actually stop this powerful beast of an algorithm... you have to let it run... till it finds the shortest path to LA, and Houston, and New York City, and everywhere possible!

Well. Once `Oakland` is popped off the priority queue in the algorithm, we can just stop. We can just return the distance and the path we have at that point, and it will be correct. So **sometimes** `dijkstra` takes in not only a source, but also a target. This is for the purposes of short-circuiting.

## The promised solutions

![](../.gitbook/assets/Screen%20Shot%202019-03-23%20at%207.29.30%20PM.png) ![](../.gitbook/assets/Screen%20Shot%202019-03-23%20at%207.30.04%20PM.png)

