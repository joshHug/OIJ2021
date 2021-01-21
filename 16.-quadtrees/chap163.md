# 16.3 K-D Trees

{% embed url="https://youtu.be/cUssdK0Tku4" caption="" %}

One way we can extend the hierarchical partitioning idea to dimensions greater than two is by using a K-D Tree. It works by rotating through all the dimensions level by level.

So for the 2-D case, it partitions like an X-based Tree on the first level, then like a Y-based Tree on the next, then as an X-based Tree on third level, a Y-based Tree on the fourth, etc.

![](../.gitbook/assets/Screen%20Shot%202019-03-16%20at%205.33.01%20PM.png) ![](../.gitbook/assets/Screen%20Shot%202019-03-16%20at%201.57.42%20AM.png)

In the first graphic, you can see how each level is partitioned. In the one below, you can see the nodes of the tree in relation to one another in the 2-D space. **Note** when you are running operations on a K-D Tree or quadtree, you should be thinking about the tree structure \(1st image\) and not the 2-D space \(2nd picture\) because only the tree holds information about the levels.

For the 3-D case, it rotates between each of the three dimensions every three levels, and so on and so forth for even higher dimensions. Here you can see the advantages in a K-D Tree in how it is more easily generalized to higher dimensions. But, no matter how high the dimensions get, a K-D tree will always be a **binary** tree, since each level is partitioned into "greater" and "less than".

For a demo on K-D tree insertion, check out these [slides](https://docs.google.com/presentation/d/1WW56RnFa3g6UJEquuIBymMcu9k2nqLrOE1ZlnTYFebg/edit#slide=id.g54b6045b73_0_38)

We can break ties by saying that items equal in one dimension should always fall the same way across the border. \(For example, an item equal in the x-dimension to its parent node will always fall to the right of the partition.\)

## Nearest Neighbor using a K-D Tree

{% embed url="https://youtu.be/mxrUFkdXaR8" caption="" %}

{% embed url="https://youtu.be/nll58oqEsBg" caption="" %}

![](../.gitbook/assets/Screen%20Shot%202019-03-16%20at%205.42.50%20PM.png)

To find the point that is the nearest neighbor to a query point, we follow this procedure in our K-D Tree:

* Start at the root and store that point as the "best so far". Compute its distance to the query point, and save that as the "score to beat". In the image above, we start at A whose distance to the flagged point is 4.5.
* This node partitions the space around it into two subspaces. For each subspace, ask: "Can a better point be possibly found inside of this space?" This question can be answered by computing the shortest distance between the query point and the edge of our subspace \(see dotted purple line below\).

![](../.gitbook/assets/Screen%20Shot%202019-03-16%20at%205.44.54%20PM.png)

* Continue recursively for each subspace identified as containing a possibly better point.
* In the end, our "best so far" is the best point; the point closest to the query point.

For a step by step walkthrough, see these [slides](https://docs.google.com/presentation/d/1DNunK22t-4OU_9c-OBgKkMAdly9aZQkWuv_tBkDg1G4/edit)

## Summary and Applications

{% embed url="https://youtu.be/ogw3Ywy8ZYM" caption="" %}

See the video above for a summary of this chapter, and some interesting applications of the presented data structures.

