# 11.3 B-Tree invariants and runtime

{% embed url="https://www.youtube.com/watch?v=DeHNhOd4xA0" caption="" %}

We mentioned in chapter 11.1 that order matters when inserting into a BST.

**Question**: Is this also true for a B-Tree?

**Exercise 11.3.1**: Insert 1-7 into a B-tree in that order. What is the height of the tree? Can we change the order of the insertions so that we can decrease the height? Here is a \[cool B-tree visualizer\]\([https://www.cs.usfca.edu/~galles/visualization/BTree.html\](https://www.cs.usfca.edu/~galles/visualization/BTree.html\)\) that might help!

**Solution**: Yes, we can get a tree of height 1 by inserting in this order: 2, 3, 4, 5, 6, 1, 7. ![](../.gitbook/assets/Screen%20Shot%202019-03-05%20at%204.35.18%20PM.png)

Yes, depending on the order you insert nodes the height of a B-tree may change. However, the tree will always be **bushy.**

A B-tree has the following helpful invariants:

* All leaves must be the same distance from the source.
* A non-leaf node with $$k$$ items must have exactly $$k+1$$ children.

In tandem, these invariants cause the tree to always be bushy.

## B-Tree runtime analysis

{% embed url="https://www.youtube.com/watch?v=Cg7k5wKGk\_Q" caption="" %}

The worst-case runtime situation for search in a B-tree would be if each node had the maximum number of elements in it and we had to traverse all the way to the bottom. We will use $$L$$ to denote the number of elements in each node. This means would would need to explore $$\log N$$ nodes \(since the max height is $$\log N$$ due to the bushiness invariant\) and at each node we would need to explore $$L$$ elements. In total, we would need to run $$L \log N$$ operations. However, we know $$L$$ is a constant, so our total runtime is $$O(\log N)$$.

## B-Tree deletion \(Extra\)

See [these extra slides](https://docs.google.com/presentation/d/1zhQDvbcDZ9RJgJl0bmqwFFlHP8ExbDFo36Q9ZWH9EgU/edit#slide=id.g508ece10b0_1_1305) if you're curious. We won't discuss them here.

### Summary

BSTs have best case height $$\Theta (\log N)$$, and worst case height $$\Theta (N)$$.

* Big O is not the same thing as worst case!

B-Trees are a modification of the binary search tree that avoids $$\Theta (N)$$ worst case.

* Nodes may contain between $$1$$ and $$L$$ items.
* contains works almost exactly like a normal BST.
* add works by adding items to existing leaf nodes.
  * If nodes are too full, they split.
* Resulting tree has perfect balance. Runtime for operations is $$O(\log N)$$.
* Have not discussed deletion. See extra slides if you’re curious.
* Have not discussed how splitting works if $$L > 3$$ \(see some other class\).
* B-trees are more complex, but they can efficiently handle ANY insertion order.

