# 11.4 Rotating Trees

Wonderfully balanced as they are, B-Trees are really difficult to implement. We need to keep track of the different nodes and the splitting process is pretty complicated. As computer scientists who appreciate good code and a good challenge, let's find another way to create a balanced tree.

## BST structure

For any BST, there are multiple ways to structure it so that you maintain the BST invariants. In chapter 11.1 we talked about how **inserting** elements in different orders will result in a different BST. The BST's below all consist of the elements 1, 2, and 3, yet all have different structures.

![](../.gitbook/assets/Screen%20Shot%202019-03-06%20at%206.53.22%20PM.png)**Exercise 11.4.1**: For each tree shown above, provide an order of insertion that yields the structure.

However, insertion is not the only way to yield different structures for the same BST. One thing we can do is change the tree with the nodes already in place through a process called **rotating**.

## Tree Rotation

The formal definition of rotation is:

`rotateLeft(G): Let x be the right child of G. Make G the new left child of x.`

`rotateRight(G): Let x be the left child of G. Make G the new right child of x.`

We will slowly demystify this process in the next few paragraphs. Below is a graphical description of what happens in a left rotation on the node G.

![](../.gitbook/assets/Screen%20Shot%202019-03-06%20at%2010.25.18%20PM.png)G's right child, P, merges with G, bringing its children along. P then passes its left child to G and G goes down to the left to become P's left child. You can see that the structure of the tree changes as well as the number of levels. We can also rotate on a non-root node. We just disconnect the node from the parent temporarily, rotate the subtree at the node, then reconnect the new root.

![](../.gitbook/assets/Screen%20Shot%202019-03-06%20at%2010.37.17%20PM.png)

Here are the implementations of `rotateRight` and `rotateLeft` courtesy of the \[princeton docs\]\([https://algs4.cs.princeton.edu/33balanced/RedBlackBST.java.html\](https://algs4.cs.princeton.edu/33balanced/RedBlackBST.java.html\)\) with some lines omitted for simplicity.

```java
private Node rotateRight(Node h) {
    // assert (h != null) && isRed(h.left);
    Node x = h.left;
    h.left = x.right;
    x.right = h;
    return x;
}

// make a right-leaning link lean to the left
private Node rotateLeft(Node h) {
    // assert (h != null) && isRed(h.right);
    Node x = h.right;
    h.right = x.left;
    x.left = h;
    return x;
}
```

Let's practice this on a few more examples.

**Exercise 11.4.2**: Give a sequence of rotations \(`rotateRight(G)`, `rotateLeft(G)`\) that will convert the tree on the left to the tree on the right.![](../.gitbook/assets/Screen%20Shot%202019-03-06%20at%2010.30.30%20PM.png)**Solution**: `rotateRight(3)`, `rotateLeft(1)`

With rotations, we can actually completely balance a tree. See the demo in these slides: [https://docs.google.com/presentation/d/1pfkQENfIBwiThGGFVO5xvlVp7XAUONI2BwBqYxib0A4/edit\#slide=id.g465b5392c\_00\](https://docs.google.com/presentation/d/1pfkQENfIBwiThGGFVO5xvlVp7XAUONI2BwBqYxib0A4/edit#slide=id.g465b5392c_00\)

In the next chapter, we will learn about a specific tree data structure that remains balanced through using rotations.

