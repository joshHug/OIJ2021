# 9.1 Introduction

Two sets are named _disjoint sets_ if they have no elements in common. A Disjoint-Sets \(or Union-Find\) data structure keeps track of a fixed number of elements partitioned into a number of _disjoint sets_. The data structure has two operations: 1. `connect(x, y)`: connect `x` and `y`. Also known as `union` 2. `isConnected(x, y)`: returns true if `x` and `y` are connected \(i.e. part of the same set\).

{% embed url="https://youtu.be/JNa8BRRs8L4" caption="" %}

A Disjoint Sets data structure has a fixed number of elements that each start out in their own subset. By calling `connect(x, y)` for some elements `x` and `y`, we merge subsets together.

For example, say we have four elements which we'll call A, B, C, D. To start off, each element is in its own set:

![](../.gitbook/assets/intro1_resized.png)

After calling `connect(A, B)`:

![](../.gitbook/assets/intro2_resized.png)

Note that the subsets A and B were merged. Let's check the output some `isConnected` calls:

`isConnected(A, B) -> true`

`isConnected(A, C) -> false`

After calling `connect(A, D)`:

![](../.gitbook/assets/intro3_resized.png)

We find the set A is part of and merge it with the set D is part of, creating one big A, B, D set. C is left alone.  
`isConnected(A, D) -> true`  
`isConnected(A, C) -> false`

With this intuition in mind, let's formally define what our DisjointSets interface looks like. As a reminder, an **interface** determines _what_ behaviors a data structure should have \(but not _how_ to accomplish it\). For now, we'll only deal with sets of non-negative integers. This is not a limitation because in production we can assign integer values to anything we would like to represent.

```java
public interface DisjointSets {
    /** connects two items P and Q */
    void connect(int p, int q);

    /** checks to see if two items are connected */
    boolean isConnected(int p, int q); 
}
```

In addition to learning about how to implement a fascinating data structure, this chapter will be a chance to see how an implementation of a data structure evolves. We will discuss four iterations of a Disjoint Sets design before being satisfied: _Quick Find → Quick Union → Weighted Quick Union \(WQU\) → WQU with Path Compression_. **We will see how design decisions greatly affect asymptotic runtime and code complexity.**

