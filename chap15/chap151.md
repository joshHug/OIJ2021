# 15.1 Introduction to Tries

We are now going to learn about a new data structure called _Tries_. These will serve as a new implementation \(from what we have previously learned\) of a _Set_ and a _Map_ that has some special functionality for certain types of data and information.

{% embed url="https://www.youtube.com/watch?v=F8Q-SHW2hAM" caption="" %}

## Reflection

Since we are thinking about the ADTs _Set_ and _Map_, let us review the implementations we've learned so far. In the past we've mainly learned how to implement these ADTs using Binary Search Trees or Hash Tables. Let's recall the runtimes for both of these:

* Balanced Search Tree:
  * `contains(x)`: $$\Theta (\log N)$$
  * `add(x)`: $$\Theta (\log N)$$
* Resizing Separate Chaining Hash Table:
  * `contains(x)`: $$\Theta(1)$$ \(assuming even spread\)
  * `add(x)`: $$\Theta(1)$$ \(assuming even spread and amortized\)

These runtimes are fantastic! We can see that the implementations for operations associated with Sets and Maps are extremely fast.

**Question:** Can we do even better than this? That could depend on the nature of our problem. What if we knew a special characteristic of the keys?

**Answer:** Yes, for example if we always know our keys are ASCII characters then instead of using a general-purpose HashMap, simply use an array where each character is a different index in our array:

```java
public class DataIndexedCharMap<V> {
    private V[] items;
    public DataIndexedCharMap(int R) {
        items = (V[]) new Object[R];
    }
    public void put(char c, V val) {
        items[c] = val;
    }
    public V get(char c) {
        return items[c];
    }
}
```

Here we have created an implementation for a map that takes in character keys. The value R represents the number of possible characters \(e.g. 128 for ASCII\). Nice! By knowing the range of our keys as well as what types of values they will be we have a simple and efficient data structure. Now we have created something for characters, what about strings?

**Exercise 15.1.1.** Come up with an implementation for a Map that contains only integer keys from 0 to 100. Ensure that this performs better than a regular HashMap.

## Inventing the Trie

Tries are a very useful data structure used in cases where keys can be broken into "characters" and share prefixes with other keys \(e.g. strings\).

Suppose we had a set containing "sam", "sad", "sap", "same", "a", and "awls. With the existing Set implementations, we have the following visual structures. How might we improve upon this using other possible data structures we know? How might we take advantage of the structure strings?

![](../.gitbook/assets/Screen%20Shot%202019-03-14%20at%2012.38.42%20AM.png)

{% embed url="https://www.youtube.com/watch?v=m42lhY5pfxE" caption="" %}

Here are some key ideas that we will use:

* Every node stores only one letter
* Nodes can be shared by multiple keys

Therefore, we can insert "sam", "sad", "sap", "same", "a", and "awls" into a tree structure that contains single character nodes. An important observation to make is that most of these words share the same _prefixes_, therefore we can utilize these similarly structured strings for our structure. In other words we don't store the same prefixes \(e.g. "sa-"\) multiple times.

Take a look at the graphic below to see how a trie would look like:

![](../.gitbook/assets/Screen%20Shot%202019-03-14%20at%2012.47.38%20AM.png)

Tries work by storing each 'character' of our keys as a node. Keys that share common prefixes share the same nodes. To check if the trie contains a key, walk down the tree from the root along the correct nodes.

Since we are going to share nodes, we must figure out some way to represent which strings belong in our set and which don't. We will solve this problem through marking the color of the last character of each string to be blue. Observe our final strategy below.

Suppose we have inserted strings into our set and we end up with the trie above, we must figure out how searching will work in our current scheme. To search, we will traverse our trie and compare to each character of the string as we go down. Thus, there are only two cases when we wouldn't be able to find a string; either the final node is white or we fall off the tree.

* `contains("sam")`: true, blue node
* `contains("sa")`: false, white node
* `contains("a")`: true, blue node
* `contains("saq")`: false, fell off tree

**Exercise 15.1.2.** Add the strings "ants", "zebra", "potato" and "sadness" to the trie above. Draw out your resulting trie structure.

**Exercise 15.1.3.** Think about the difference between a trie being used as a map versus a trie being used as a set. What about \(if any\) the implementation would be different?

See an animated demo of creation of a trie map [here](http://www.cs.princeton.edu/courses/archive/spring15/cos226/demo/52DemoTrie.mov).

## Summary

A key takeaway is that we can often improve a general-purpose data structure when we add specificity to our problem, often by adding additional constraints. For example, we improved our implementation of HashMap when we restricted the keys to only be ASCII character, creating extremely efficient data structure.

* There is a distinction between ADTs and specific implementations.As an example, Disjoint Sets is an ADT: any Disjoint Sets has the methods `connect(x, y)` and `isConnected(x, y)`. There are four different ways to _implement_ those methods: Quick Find, Quick Union, Weighted QU, and WQUPC.
* The Trie is a specific implementation for Sets and Maps that is specialized for strings.
  * We give each node a single character and each node can be a part of several keys inside of the trie.
  * Searching will only fail if we hit an unmarked node or we fall off the tree
  * Short for Re**trie**val tree, almost everyone pronounces it as "try" but Edward Fredkin suggested it be pronounced as "tree"

