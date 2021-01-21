# 11.5 Red-Black Trees

We said in the previous section that we really like 2-3 trees because they always remain balanced, but we also don't like them because they are hard to implement. But why not both? Why not create a tree that is implemented using a BST, but is structurally identical to a 2-3 tree and thus stays balanced? \(Note that in this chapter we will be honing in on 2-3 Trees specifically, not 2-3-4 trees\)

**Enter the Red-Black Tree**

We are going to create this tree by looking at a 2-3 tree and asking ourselves what kind of modifications we can make in order to convert it into a BST.

For a 2-3 tree that only has 2-nodes \(nodes with 2 children\), we already have a BST, so we don't need to make any modifications!

However, what happens when we get a 3-node?

One thing we could do is create a "glue" node that doesn't hold any information and only serves to show that its 2 children are actually a part of one node.![](../.gitbook/assets/Screen%20Shot%202019-03-06%20at%2010.51.15%20PM.png)

However, this is a very inelegant solution because we are taking up more space and the code will be ugly. So, instead of using glue nodes we will use glue links instead!

![](../.gitbook/assets/Screen%20Shot%202019-03-06%20at%2010.56.51%20PM.png)We choose arbitrarily to make the left element a child of the right one. This results in a **left-leaning** tree. We show that a link is a glue link by making it red. Normal links are black. Because of this, we call these structures **left-leaning red-black trees \(LLRB\)**. We will be using left-leaning trees in 61B.

### Left-Leaning Red-Black trees have a 1-1 correspondence with 2-3 trees. Every 2-3 tree has a unique LLRB red-black tree associated with it. As for 2-3-4 trees, they maintain correspondence with standard Red-Black trees.

## Properties of LLRB's

Here are the properties of LLRB's:

* 1-1 correspondence with 2-3 trees.
* No node has 2 red links.
* There are no red right-links.
* Every path from root to leaf has same number of black links \(because 2-3 trees have same number of links to every leaf\).
* Height is no more than 2x height of corresponding 2-3 tree.

## Inserting into LLRB

We can always insert into a LLRB tree by inserting into a 2-3 tree and converting it using the scheme from above. However, this would be contrary to our original purpose of creating a LLRB, which was to avoid the complicate code of a 2-3 tree! Instead, we insert into the LLRB as we would with a normal BST. However, this could break its 1-1 mapping to a 2-3 tree, so we will use rotations to massage the tree back into a proper structure.

We will go over the different tasks we need to address when inserting into a LLRB below.

1. **Task 1: insertion color**: because in a 2-3 tree, we are always inserting by adding to a leaf node, the color of the link we add should always be red.
2. **Task 2: insertion on the right**: recall, we are using _left-leaning_ red black trees, which means we can never have a right red link. If we insert on the right, we will need to use a rotation in order to maintain the LLRB invariant.![](../.gitbook/assets/Screen%20Shot%202019-03-06%20at%2011.14.41%20PM.png)  
   However, if we were to insert on the right with a red link and the left child is _also_ a red link, then we will temporarily allow it for purposes that will become clearer in task 3.

   ![](../.gitbook/assets/Screen%20Shot%202019-03-06%20at%2011.33.14%20PM.png)

3. **Task 3: double insertion on the left:** If there are 2 left red links, then we have a 4-node which is illegal. First, we will rotate to create the same tree seen in task 2 above. ![](../.gitbook/assets/Screen%20Shot%202019-03-06%20at%2011.36.00%20PM.png)Then, in both situations, we will flip the colors of all edges touching S. This is equivalent to pushing up the middle node in a 2-3 tree.

![](../.gitbook/assets/Screen%20Shot%202019-03-06%20at%2011.37.57%20PM.png)

You may need to go through a series of rotations in order to complete the transformation. The process is: while the LLRB tree does not satisfy the 1-1 correspondence with a 2-3 tree or breaks the LLRB invariants, perform task 1, 2, or 3 depending on the condition of the tree until you get a legal LLRB.

Here is a summary of all the operations:

* When inserting: Use a red link.
* If there is aright leaning “3-node”, we have a Left Leaning Violation
  * Rotate left the appropriate node to fix.
* If there are two consecutive left links, we have an incorrect 4 Node Violation!
  * Rotate right the appropriate node to fix.
* If there are any nodes with two red children, we have a temporary 4 Node.
  * Color flip the node to emulate the split operation.

## Runtime

Because a left-leaning red-black tree has a 1-1 correspondence with a 2-3 tree and will always remain within 2x the height of its 2-3 tree, the runtimes of the operations will take $$\log N$$ time.

Here's the abstracted code for insertion into a LLRB:

```java
private Node put(Node h, Key key, Value val) {
    if (h == null) { return new Node(key, val, RED); }

    int cmp = key.compareTo(h.key);
    if (cmp < 0)      { h.left  = put(h.left,  key, val); }
    else if (cmp > 0) { h.right = put(h.right, key, val); }
    else              { h.val   = val;                    }

    if (isRed(h.right) && !isRed(h.left))      { h = rotateLeft(h);  }
    if (isRed(h.left)  &&  isRed(h.left.left)) { h = rotateRight(h); }
    if (isRed(h.left)  &&  isRed(h.right))     { flipColors(h);      } 

    return h;
}
```

Look how short and sweet!

## Summary

* Binary search trees are simple, but they are subject to imbalance which leads to crappy runtime.
* 2-3 Trees \(B Trees\) are balanced, but painful to implement and relatively slow.
* LLRBs insertion is simple to implement \(but deletion is hard\).
  * Works by maintaining mathematical bijection with a 2-3 trees.
* Java’s [TreeMap](https://github.com/AdoptOpenJDK/openjdk-jdk11/blob/999dbd4192d0f819cb5224f26e9e7fa75ca6f289/src/java.base/share/classes/java/util/TreeMap.java) is a red-black tree \(but not left leaning\).
* LLRBs maintain correspondence with 2-3 tree, Standard Red-Black trees maintain correspondence with 2-3-4 trees.
* Allows glue links on either side \(see [Red-Black Tree](http://en.wikipedia.org/wiki/Red–black_tree)\).
* More complex implementation, but significantly faster.

