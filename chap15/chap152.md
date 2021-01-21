# 15.2 Implementation and Performance

## Implementation

{% embed url="https://www.youtube.com/watch?v=DqfZ4BEVDgk" caption="" %}

Let's actually try building a Trie. We'll take a first approach with the idea that each node stores a letter, its children, and a color. Since we know each node key is a character, we can use our `DataIndexedCharMap` class we defined earlier to map to all of a nodes' children. Remember that each node can have at most the number of possible characters as its number of children.

```java
public class TrieSet {
   private static final int R = 128; // ASCII
   private Node root;    // root of trie

   private static class Node {
      private char ch;  
      private boolean isKey;   
      private DataIndexedCharMap next;

      private Node(char c, boolean blue, int R) {
         ch = c; 
         isKey = blue;
         next = new DataIndexedCharMap<Node>(R);
      }
   }
}
```

Zooming in on a single node with one child we can observe that its `next` variable, the DataIndexedCharMap object, will have mostly null links if nodes in our tree have relatively few children. We will have 128 links with 127 equal to null and 1 being used. This means that we are wasting a lot of excess space! We will explore alternative representations further on.

But we can make an important observation: each link corresponds to a character if and only if that character **exists**. Therefore, we can remove the Node's character variable and instead base the value of the character from its position in the parent `DataIndexedCharMap`.

```java
public class TrieSet {
   private static final int R = 128; // ASCII
   private Node root;    // root of trie

   private static class Node {
      private boolean isKey;   
      private DataIndexedCharMap next;

      private Node(boolean blue, int R) {
         isKey = blue;
         next = new DataIndexedCharMap<Node>(R);
      }
   }
}
```

**Exercise 15.2.1.** Come up with a solution to the excess use of space. _Hint_: Try using some of the implementations we have discussed before.

## Performance

Given a Trie with N keys the runtime for our Map/Set operations are as follows:

* `add`: $$\Theta (1)$$
* `contains`: $$\Theta (1)$$

Why is this the case? It doesn't matter how many items we have in our trie, the runtime will always be independent of the number of keys. This is because we only traverse the length of one key in the worst case ever, which is never related to the number of keys in the trie. Therefore, let's look at the runtime through a measurement that can be measured; in terms of L, the length of the key:

* `add`: $$\Theta(L)$$
* `contains`: $$O(L)$$

We have achieved constant runtime without having to worry about amortized resizing times or an even spreading of keys, but as we mentioned above our current design is extremely wasteful since each node contains an array for every single character even if that character doesn't exist.

## Child Tracking

{% embed url="https://www.youtube.com/watch?v=NTdJQB8yr2I" caption="" %}

The problem we were running into was waste of space from our implementation of a DataIndexedCharMap object to track each node's children. The problem with this approach was that we would have initialized many **null** spots that don't contain any children.

* _Alternate Idea \#1_: Hash-Table based Trie.  This won't create an array of 128 spots, but instead initialize the default value and resize the array only when necessary with the load factor.  
* _Alternate Idea \#2_: BST based Trie.  Again this will only create children pointers when necessary, and we will store the children in the BST.  Obviously, we still have the worry of the runtime for searching in this BST, but this is not a bad approach.

When we implement a Trie, we have to pick a map to our children. A Map is an ADT, so we must also choose the underlying implementation for the map. What does this reiterate to us? There is an **abstraction** barrier between the implementations and the ADT that we are trying to create. This abstraction barrier allows us to take advantage of what each implementation has to offer when we try to meet the ADT behavior. Let's consider each advantage:

* DataIndexedCharMap 
  * Space: 128 links per node
  * Runtime: $$\Theta(1)$$
* BST
  * Space: C links per node, where C is the number of children
  * Runtime: $$O(\log R)$$, where R is the size of the alphabet
* Hash Table
  * Space: C links per node, where C is the number of children
  * Runtime: $$O(R)$$, where R is the size of the alphabet

Note: Cost per link is higher in BST and Hash Tables; R is a fixed number \(this means we can think of the runtimes as constant\)

We can takeaway a couple of things. There is a slight memory and efficiency trade off \(with BST/Hash Tables vs. DataIndexedCharMap\). The runtimes for Trie operations are still constant without any caveats. Tries will especially thrive with some special operations.

