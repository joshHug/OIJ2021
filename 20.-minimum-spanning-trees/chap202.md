# 20.2 Prim's and Kruskal's

## Prim's Algorithm

{% embed url="https://youtu.be/ZCMTccvfaTQ" caption="" %}

{% embed url="https://youtu.be/JoS9ZegarJs" caption="" %}

This is one algorithm to find a MST from a graph. It is as follows: 1. Start from some arbitrary start node. 2. Repeatedly add the shortest edge that has one node inside the MST under construction. 3. Repeat until there are V-1 edges.

Prim's algorithm works because at all stages of the algorithm, if we take all the nodes that are part of our MST under construction as one set, and all other nodes as a second set, then this algorithm always adds the lightest edge that crosses this cut, which is necessarily part of the final MST by the Cut Property.

{% embed url="https://youtu.be/gurX\_3NRPS8" caption="" %}

Essentially, this algorithm runs via the same mechanism as Dijkstra's algorithm, but while Dijkstra's considers candidate nodes by their distance from the source node, Prim's looks at each candidate node's distance from the MST under construction.

Thus, the runtime of Prim's if done using the same mechanism as Dijkstra's, would be the same as Dijkstra's, which is $$O((|V|+|E|) \log |V|)$$. Remember, this is because we need to add to a priority queue fringe once for every edge we have, and we need to dequeue from it once for every vertex we have.

## Kruskal's Algorithm

{% embed url="https://youtu.be/hSf\_jir40ho" caption="" %}

{% embed url="https://youtu.be/vmWSnkBVvQ0" caption="" %}

{% embed url="https://youtu.be/4TV-b64HNaA" caption="" %}

This is another algorithm that can be used to find a MST from a graph. The MST returned by Kruskal's might not be the same one returned by Prim's, but both algorithms will always return a MST; since both are minimal \(optimal\), they will both give valid optimal answers \(they are tied as equally minimal / same total weight, and this is as low as it can be\).

The algorithm is as follows: 1. Sort all the edges from lightest to heaviest. 2. Taking one edge at a time \(in sorted order\), add it to our MST under construction if doing so does not introduce a cycle. 3. Repeat until there are $$V-1$$ edges.

Kruskal's algorithm works because any edge we add will be connecting one node, which we can say is part of one set, and a second node, which we can say is part of a second set. This edge we add is not part of a cycle, because we are only adding an edge if it does not introduce a cycle. Further, we are looking at edge candidates in order from lightest to heaviest. Therefore, this edge we are adding must be the lightest edge across this cut \(if there was a lighter edge that would be across this cut, it would have been added before this, and adding this one would cause a cycle to appear\). Therefore, this algorithm works by the Cut Property as well.

Kruskal's runs in $$O(|E| \log |E|)$$ time because the bottleneck of the algorithm is sorting all of the edges to start \(for example, we can use heap sort, in which we insert all of the edges into a heap and remove the min one at a time\). If we are given pre-sorted edges and don't have to pay for that, then the runtime is $$O(|E| \log^* |V|)$$. This is because with every edge we propose to add, we need to check whether it will introduce a cycle or not. One way we know how to do this is by using Weighted Quick Union with Path Compression; this will efficiently tell us whether two nodes are connected \(unioned\) together already or not. This will cost $$|E|$$ calls on isConnected, which costs $$O(\log^* |V|)$$ each, where $$\log^*$$ is the Ackermann function.

{% embed url="https://youtu.be/OetLdLoEbKQ" caption="" %}

