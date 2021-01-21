# 11.1 Intro to B-Trees

## Binary Tree Height

{% embed url="https://www.youtube.com/watch?v=0SCtnf84QrI" caption="" %}

The difference in runtime between a worst-case tree and best-case tree is very dramatic.

**Worst case:** $$\Theta(N)$$

**Best-case:** $$\Theta(\log N)$$ \(where $$N$$ is number of nodes in the tree\)

The runtimes are dependent on the structure of the tree. If the tree is really spindly, then its basically a linked list and the runtime is linear. If the tree is bushy, then the height of the tree is $$\log N$$ and therefore the runtime grows in $$\log N$$ time.

![](../.gitbook/assets/Screen%20Shot%202019-03-05%20at%2012.56.54%20PM.png)

### A short detour into BigO and worst case

BigO is **not** equivalent to worst case! Remember, BigO is an upper bound. As long as a function falls within that bound, it is considered to be inside the BigO of that function. Worst-case is more restrictive than BigO.

As an example, think about searching for hotels. If you are searching for a hotel with worst-case $500 rooms, then only a few hotels would fit that requirement, perhaps only the ritz carlton. On the other hand, if you were searching for hotels that are in O\(500\) or less than $500 rooms, then many hotels would fall under that category from the Motel 6 to the Rodeway Inn to the Ritz Carlton.

Thus, even though we said the worst-case runtime of a BST is $$\Theta(N)$$, it also falls under $$O(N^2)$$.

Many people use BigO as shorthand for worst-case, but this is technically not synonymous. We want you guys to be cream-of-the-crop computer scientists, so we are making this distinction!

## BST Performance

{% embed url="https://www.youtube.com/watch?v=yz850zzjrHQ" caption="" %}

Some terminology for BST performance:

* **depth**: the number of links between a node and the root.
* **height**: the lowest depth of a tree.
* **average depth**: average of the total depths in the tree. You calculate this by taking $$\frac{\sum_{i=0}^D{d_in_i}}{N}$$ where $$d_i$$ is depth and $$n_i$$ is number of nodes at that depth.

The **height** of the tree determines the worst-case runtime, because in the worst case the node we are looking for is at the bottom of the tree.

The **average depth** determines the average-case runtime.

### BST insertion order

The order you insert nodes into a BST determines its height.

**Exercise 11.1.1** Given integers 1-7, what order of inserting would result in the worst-case height? What order would result in the best case?

**Answer** To get the worst-case height you can just insert in order from 1-7. To get the best case height insert 4 first, then 2 and 6, then 1, 3, 5, 7. Convince yourself of this by actually going through the insertion process.

![](../.gitbook/assets/Screen%20Shot%202019-03-05%20at%203.53.32%20PM.png)

Yes, having a specific insertion order can help us get a balanced tree. However, we don't even need to be that intentional. If we just insert the nodes in **random** order, it will actually end up being relatively bushy!

{% embed url="https://www.youtube.com/watch?time\_continue=3&v=5dGkblzqdmc" caption="" %}

You don't have to know the proof of this, but when we insert randomly into a BST the **average depth** and **height** are expected to be $$\Theta(log N)$$.

However, we won't always be able to insert into a BST in random order. What if our data comes in real-time? Then, we will be forced to insert in the order that data comes to us.

In the next chapter we will learn about a tree that always maintains its balance!

