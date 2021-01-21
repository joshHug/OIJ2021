# 10.2 Trees

Now we are going to learn about perhaps the most important data structure ever.

{% embed url="https://www.youtube.com/watch?v=AcRKQOe0zYg&list=PL8FaHk7qbOD5JBcKwLbuUf6BAdotM4-1K&index=2" caption="" %}

Linked Lists are great, but it takes a long time to search for an item, even if the list is sorted! What if the item is at the end of the list? That would take linear time! Take a look at the linked list below and convince yourself that this is true.

![](../.gitbook/assets/Screen%20Shot%202019-02-28%20at%2012.55.40%20AM.png)

We know that for an array, we can use binary search to find an element faster. Specifically, in $$\log (n)$$ time. For a short explanation of binary search, check out this [link](https://www.geeksforgeeks.org/binary-search/).

TL;DR: In binary search, we know the list is sorted, so we can use this information to narrow our search. First, we look at the middle element. If it is bigger than the element we are searching for, we look to the left of it. If it is smaller than the element we are searching for, we look to the right. We then look at the middle element of the respective halves and repeat the process until we find the element we are looking for \(or don't find it because the list doesn't contain it\).

But how do we run binary search for a linked list? We would have to traverse all the way to the middle in order to check the element there, which takes linear time just on its own!

One optimization we can implement is to have a reference to the middle node. This way, we can get to the middle in constant time. Then, if we flip the nodes' pointers, which allows us to traverse to both the left and right halves, we can decrease our runtime by half!

![](../.gitbook/assets/Screen%20Shot%202019-02-28%20at%2012.51.06%20AM.png)

But, we can do better than that. We can further optimize by adding pointers to the middle of each recursive half like so.

![](../.gitbook/assets/Screen%20Shot%202019-02-28%20at%2012.58.07%20AM.png)

Now, if you stretch this structure vertically, you will see a tree!

![](../.gitbook/assets/Screen%20Shot%202019-02-28%20at%2012.59.39%20AM.png)

This specific tree is called a **binary tree** because each juncture splits in 2.

### Properties of trees

Let's formalize the tree data structure a bit more.

{% embed url="https://www.youtube.com/watch?v=slOxliXDV-s&index=3&list=PL8FaHk7qbOD5JBcKwLbuUf6BAdotM4-1K" caption="" %}

Trees are composed of:

* nodes
* edges that connect those nodes.
  * **Constraint**: there is only one path between any two nodes.

In some trees, we select a **root** node which is a node that has no parents.

A tree also has **leaves**, which are nodes with no children.

The below structures are valid trees: ![](../.gitbook/assets/Screen%20Shot%202019-02-28%20at%209.25.43%20AM.png)

**Exercise 10.2.1:** Can you come up with an example of a non-valid tree?

Relating this to the original tree structure we came up with earlier, we can now introduce new constraints to the already existing constraints. This creates more specific types of trees, two examples being Binary Trees and Binary Search Trees.

* **Binary Trees**: in addition to the above requirements, also hold the binary property constraint. That is, each node has either 0, 1, or 2 children.
* **Binary Search Trees**: in addition to all of the above requirements, also hold the property that For every node X in the tree:
  * Every key in the left subtree is less than X’s key.
  * Every key in the right subtree is greater than X’s key.

    \*\*Remember this property!! We will reference it a lot throughout the duration of this module and 61B.

![](../.gitbook/assets/Screen%20Shot%202019-02-28%20at%209.36.01%20AM.png)

Here is the BST class we will be using in this module:

```java
private class BST<Key> {
    private Key key;
    private BST left;
    private BST right;

    public BST(Key key, BST left, BST Right) {
        this.key = key;
        this.left = left;
        this.right = right;
    }

    public BST(Key key) {
        this.key = key;
    }
}
```

## Binary Search Tree Operations

{% embed url="https://www.youtube.com/watch?v=PLyDf3\_J7Cc&index=4&list=PL8FaHk7qbOD5JBcKwLbuUf6BAdotM4-1K" caption="" %}

### Search

To search for something, we employ binary search which is made easy due to the BST property described in the previous section!

We know that the BST is structured such that all elements to the right of a node are greater and all elements to the left are smaller. Knowing this, we can start at the root node and compare it with the element, X, that we are looking for. If X is greater to the root, we move on to the root's right child. If its smaller, we move on to the root's left child. We repeat this process recursively until we either find the item or we get to a leaf in which case the tree does not contain the item.

**Exercise 10.2.2:** Try to write this method by yourself. Here is the method header: `static BST find(BST T, Key key)`. It should return the BST rooted at the node whose key matched the key parameter.

```java
static BST find(BST T, Key sk) {
   if (T == null)
      return null;
   if (sk.equals(T.key))
      return T;
   else if (sk ≺ T.key)
      return find(T.left, sk);
   else
      return find(T.right, sk);
}
```

If our tree is relatively "bushy", the find operation will run in $$\log (n)$$ time because the height of the tree is logn, that's pretty fast!

### Insert

{% embed url="https://www.youtube.com/watch?v=otDvoMb8UqE&list=PL8FaHk7qbOD5JBcKwLbuUf6BAdotM4-1K&index=5" caption="" %}

We **always** insert at a leaf node!

First, we search in the tree for the node. If we find it, then we don't do anything. If we don't find it, we will be at a leaf node already. At this point, we can just add the new element to either the left or right of the leaf, preserving the BST property.

**Exercise 10.2.3:** Try to write this method by yourself. Here is the method header: `static BST insert(BST T, Key ik)`. It should return the full BST with the new node inserted in the correct position.

```java
static BST insert(BST T, Key ik) {
  if (T == null)
    return new BST(ik);
  if (ik ≺ T.key)
    T.left = insert(T.left, ik);
  else if (ik ≻ T.key)
    T.right = insert(T.right, ik);
  return T;
}
```

**Exercise 10.2.4:** Think of an order of insertions that would result in differing heights of trees. Try to find the two extreme cases for the height of a tree. Hint: Your first insertion will determine much of the behavior for the following insertions.

### Delete

{% embed url="https://www.youtube.com/watch?v=vPzB6svl4rc&list=PL8FaHk7qbOD5JBcKwLbuUf6BAdotM4-1K&index=6" caption="" %}

Deleting from a binary tree is a little bit more complicated because whenever we delete, we need to make sure we reconstruct the tree and still maintain its BST property.

Let's break this problem down into three categories:

* the node we are trying to delete has no children
* has 1 child
* has 2 children

**No children**

If the node has no children, it is a leaf, and we can just delete its parent pointer and the node will eventually be swept away by the [garbage collector](https://stackoverflow.com/questions/3798424/what-is-the-garbage-collector-in-java).

![](../.gitbook/assets/Screen%20Shot%202019-02-28%20at%2010.34.53%20AM.png)

**One child**

If the node only has one child, we know that the child maintains the BST property with the parent of the node because the property is recursive to the right and left subtrees. Therefore, we can just reassign the parent's child pointer to the node's child and the node will eventually be garbage collected.

![](../.gitbook/assets/Screen%20Shot%202019-02-28%20at%2010.35.56%20AM.png)

**Two children**

If the node has two children, the process becomes a little more complicated because we can't just assign one of the children to be the new root. This might break the BST property.

Instead, we choose a new node to replace the deleted one.

We know that the new node must:

* be &gt; than everything in left subtree.
* be &lt; than everything right subtree.

In the below tree, we show which nodes would satisfy these requirements given that we are trying to delete the `dog` node.

![](../.gitbook/assets/Screen%20Shot%202019-02-28%20at%2010.36.39%20AM.png)

To find these nodes, you can just take the right-most node in the left subtree or the left-most node in the right subtree.

Then, we replace the `dog` node with either `cat` or `elf` and then remove the old `cat` or `elf` node.

This is called **Hibbard deletion**, and it gloriously maintains the BST property amidst a deletion.

![](../.gitbook/assets/Screen%20Shot%202019-02-28%20at%2010.43.45%20AM.png)

## BSTs as Sets and Maps

{% embed url="https://www.youtube.com/watch?v=sL2p1slgUMg&list=PL8FaHk7qbOD5JBcKwLbuUf6BAdotM4-1K&index=7" caption="" %}

We can use a BST to implement the `Set` ADT! But its even better because in an ArraySet, we have worst-case $$O(n)$$ runtime to run `contains` because we need to search the entire set. However, if we use a BST, we can decrease this runtime to $$\log (n)$$ because of the BST property which enables us to use binary search!

We can also make a binary tree into a map by having each BST node hold `(key,value)` pairs instead of singular values. We will compare each element's key in order to determine where to place it within our tree.

## Summary

Abstract data types \(ADTs\) are defined in terms of operations, not implementation.

Several useful ADTs:

* Disjoint Sets, Map, Set, List.
* Java provides Map, Set, List interfaces, along with several implementations.

We’ve seen two ways to implement a Set \(or Map\):

* ArraySet: $$\Theta(N)$$ operations in the worst case.
* BST: $$\Theta(\log N)$$ operations if tree is balanced.

BST Implementations:

* Search and insert are straightforward \(but insert is a little tricky\).
* Deletion is more challenging. Typical approach is “Hibbard deletion”.

#### What Next

* [Lab 7](https://sp19.datastructur.es/materials/lab/lab7/lab7)
* [Discussion 7](https://sp19.datastructur.es/materials/discussion/disc07.pdf)

