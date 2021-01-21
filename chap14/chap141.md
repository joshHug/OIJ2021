# 14.1 Data Structures Summary

{% embed url="https://www.youtube.com/watch?v=i-OuY5o\_G8g&t=317s" caption="" %}

## The Search Problem

The problem we are presented: Given a stream of data, retrieve information of interest.

What are some examples of this?

* Website users post to personal page. Serve content only to friends.
* Given logs for thousands of weather stations, display weather map for specified date and time.
* Dog owners request the best pet store, choosing to define their best store either in price, quality, or atmosphere.

All of the data structures we have discussed so far have been to _solve_ the search problem. How you might ask? Each of the data structures we've learned are used for storing information in schemes that make searching efficient in specific scenarios.

### Search Data Structures

| Name | Store Operation\(s\) | Primary Retrieval Operation | Retrieve By |
| :--- | :--- | :--- | :--- |
| List | `add(key)`, `insert(key, index)` | `get(index)` | index |
| Map | `put(key, value)` | `get(key)` | key identity |
| Set | `add(key)` | `containsKey(key)` | key identity |
| PQ | `add(key)` | `getSmallest()` | key order \(aka key size\) |
| Disjoint Sets | `connect(int1, int2)` | `isConnected(int1, int2)` | two integer values |

Remember that these are **Abstract** Data Types. This means that we define the behavior, not the implementation. We've defined many of the possible implementations in previous chapters. Let's think about how these implementations and ADTs interact:

![](../.gitbook/assets/Screen%20Shot%202019-03-13%20at%2011.37.10%20PM.png)

What does this diagram tell us? The first thing you'll notice is that many implementations that we have devised in earlier chapters can be used in implementing many different ADTs. You'll also notice the red colored implementations \(meaning poor performance\), telling us that not all implementations are optimal for the behavior we are trying to achieve.

**Exercise 14.1.1.** Consider how each of these implementations can be modified in order to accommodate to the behavior attempting to be defined. For example, how would we use a Hash Table to function as a Priority Queue?

## Abstraction

Abstraction often happens in layers. Abstract Data Types can often contain two abstract ideas boiling down to one implementation. Let's consider some examples:

* If we remembered the Priority Queue ADT, we were attempting to find an implementation that would be efficient for PQ operations.  We decided that our Priority Queue would be implemented using a Heap Ordered Tree, but as we saw we had several approaches \(1A, 1B, 1C, 2, 3\) of representing a tree for heaps.
* A similar idea is an External Chaining Hash Table.  This data structure is implemented using an array of buckets, but these buckets can be done using either an ArrayList, Resizing Array, Linked List, or BST.  

These two examples tell us that we can often think of an ADT by the use of another ADT. And that Abstract Data Types have layers of abstraction, each defining a behavior that is more specific than the idea that came before it.

