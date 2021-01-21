# 11.2 B-Trees

{% embed url="https://www.youtube.com/watch?v=-ECGVvUHA5c" caption="" %}

The problem with BST's is that we always insert at a leaf node. This is what causes the height to increase. Take this nice balanced tree below:

![](../.gitbook/assets/Screen%20Shot%202019-03-05%20at%204.02.58%20PM.png)

When we start inserting nodes, we could potentially break the balanced structure. So, let's come up with a way to keep the tree balanced when we add new nodes!

**Crazy Idea**: let's just never add a leaf node! When we insert, let's just add to a current leaf node. This way, the height will never increase.

![](../.gitbook/assets/Screen%20Shot%202019-03-05%20at%204.05.03%20PM.png)

However, could you see a potential problem with this insertion scheme? If we search for 19, then we will traverse down to the node that contains it, and we still have to look through that node as if we are looking through an array in order to get to the 19 element. This will lead to a runtime of $$N$$ again!

**Solution**: Set a limit on the number of elements in a single node. Let's say 4. If we need to add a new element to a node when it already has 4 elements, we will split the node in half. by bumping up the middle left node.

![](../.gitbook/assets/Screen%20Shot%202019-03-05%20at%204.12.17%20PM.png)

Notice that now the 15-17 node has 3 children now, and each child is either less than, between, or greater than 15 and 17. By keeping the children sorted, we can continue to use binary search.

By splitting nodes in the middle, we maintain perfect balance! These trees are called **B-trees** or **2-3-4/2-3 Trees**. 2-3-4 and 2-3 refer to the number of children each node can have. So, a 2-3-4 tree can have 2, 3, or 4 children while a 2-3 tree can have 2 or 3. This means that 2-3-4 trees split nodes when they have 3 nodes and one more needs to be added. 2-3 trees split after they have 2 nodes and one more needs to be added.

## Insertion Process

The process of adding a node to a 2-3-4 tree is:

1. We still always inserting into a leaf node, so take the node you want to insert and traverse down

   the tree with it, going left and right according to whether or not the node to be inserted

   is greater than or smaller than the items in each node.  

2. After adding the node to the leaf node, if the new node has 4 nodes, then pop up the middle left node and re-arrange the children accordingly. 
3. If this results in the parent node having 4 nodes, then pop up the middle left node again, rearranging the children accordingly. 
4. Repeat this process until the parent node can accommodate or you get to the root.

For a 2-3 tree, go through the same process except push up the middle node in a 3-element node.

If you want some more practice, go through the exercises in these [slides](https://docs.google.com/presentation/d/1zhQDvbcDZ9RJgJl0bmqwFFlHP8ExbDFo36Q9ZWH9EgU/edit#slide=id.g508ece10b0_1_584)

