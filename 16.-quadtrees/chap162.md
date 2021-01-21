# 16.2 QuadTrees

## Third Attempt: QuadTrees

### X-Based Tree or Y-Based Tree

{% embed url="https://youtu.be/VC1kZ42XCkY" caption="" %}

One key advantage of Search Trees over Hash Tables is that trees explicitly track the order of items. For example, finding the minimum item in a BST is $$\Theta(\log N)$$ time, but $$\Theta (N)$$ in a hash table. Let's try to leverage that to our advantage here to give us better performance for our motivating goals.

This isn't trivial though...in order to build a Binary Search Tree, we need to be able to compare objects. However, in two \(or more\) dimensional space, one object might be "less than" another in one dimension, but "greater than" the other in the other dimension. So which should be the "lesser" and "greater" for the purposes of our search tree?

For example, below Mars is "less than" Earth in the x-dimension, but "greater than" Earth in the y-dimension.

![](../.gitbook/assets/Screen%20Shot%202019-03-15%20at%2011.32.09%20AM.png)

So which of these two representations shown below should we use? Remember that we don't want to tie-break arbitrarily because we need to be able to know for certain that a node is going to be down one particular path or not in the tree at all. Otherwise, we may lose our $$\log N$$ runtime.

![](../.gitbook/assets/Screen%20Shot%202019-03-15%20at%2011.32.13%20AM.png)

Say we use the X-based Tree--that is--we construct a BST looking only at the x-coordinate. \(We ignore the y-coordinate when organizing it.\) For a larger example, we might get something like this:

![](../.gitbook/assets/Screen%20Shot%202019-03-15%20at%2011.40.56%20AM.png)

![](../.gitbook/assets/Screen%20Shot%202019-03-15%20at%2011.41.02%20AM.png)

Notice that if we are performing a search on this tree, and we're looking for a point that has an x-coordinate less than $$-1$$, from the root when we choose to take the left path, we immediately get to discard everything in the right subtree. And this is analogous to saying that we have been able to restrict our search space from the entire image space, to just the green rectangle. The ability to skip searching through parts of your search tree is called "pruning".

However on the flip side, if we were looking for a point that has a particular y-coordinate, our X-based tree is not optimal for that kind of search and we'd have to perform a linear search on all the nodes.

No matter whether we choose the X-based tree representation or the Y-based tree representation, we will always have suboptimal pruning; search in the optimized dimension will be $$\log N$$, but search in the non-optimized dimension will be $$N$$ in runtime.

### QuadTree

{% embed url="https://youtu.be/vGRyb1fK-bg" caption="" %}

We can solve this problem by splitting in both directions simultaneously. This is the QuadTree.

![](../.gitbook/assets/Screen%20Shot%202019-03-16%20at%201.33.04%20AM.png)

![](../.gitbook/assets/Screen%20Shot%202019-03-16%20at%201.33.08%20AM.png)

Here, we see that node A splits its surrounding area into a northwest, northeast, southeast, and southwest region. Since B resides in the northeast quadrant of A, when we insert B, we can put it as a child of A as its NE child.

Note that just like in a BST, the order in which we insert nodes determines the topology of the QuadTree.

Also note that QuadTrees are a form of spatial partitioning in disguise. Similar to how uniform partitioning created a perfect grid before, QuadTrees hierarchically partition by having each node "own" 4 subspaces.

Effectively, spaces where there are many points are broken down into more finely divided regions, and in many cases this gives better performances.

### Range Search using a QuadTree

{% embed url="https://youtu.be/D6nrGYfnWFI" caption="" %}

Notice that with the 4-way division imposed by each node of the QuadTree, we still have the pruning effect that was so advantageous in our X-Based Tree and Y-Based Tree! If we are looking for points inside a green rectangle as shown below, from any node we can decide whether the green rectangle lies within one or more quadrants, and only explore the branches/subtrees corresponding to those quadrants. All other quadrants can be safely ignored and pruned away. Below, we see that the green rectangle lies only in the northeast quadrant, and so the NW, SE, and SW quadrants can all be pruned away and left unexplored. We can proceed recursively.

![](../.gitbook/assets/Screen%20Shot%202019-03-16%20at%201.45.55%20AM.png)

![](../.gitbook/assets/Screen%20Shot%202019-03-16%20at%201.46.01%20AM.png)

Quad-Trees are great for 2-D spaces, because there are only 4 quadrants. However, what do we do if we want to move into higher dimension space? We'll explore another data structure in the next chapter that is equipped to tackle this question.

