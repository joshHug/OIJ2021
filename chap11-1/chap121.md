# 12.1 A first attempt

## Issues with what we've seen so far

So far, we've looked at a few data structures for efficiently searching for the existence of items within the data structure. We looked at Binary Search Trees, then made them balanced using 2-3 Trees.

However, there are some limitations that these structures impose \(yes, even 2-3 trees!\)

1. They require that items be comparable. How do you decide where a new item goes in a BST? You have to answer the question "are you smaller than or bigger than the root"? For some objects, this question may make no sense. 
2. They give a complexity of $$\Theta(\log N)$$. Is this good? Absolutely. But maybe we can do better. 

### A first attempt: `DataIndexedIntegerSet`

Let us begin by considering the following approach.

For now, we're only going to try to improve issue \#2 above \(improve complexity from $$\Theta(\log N)$$ to $$\Theta(1)$$. We're going to not worry about issue \#1 \(comparability\). In fact, we're going to only consider storing and searching for `int`s.

Here's an idea: let's create an ArrayList of type `boolean` and size 2 billion. Let everything be false by default.

* The `add(int x)` method simply sets the `x` position in our ArrayList to true. This takes $$\Theta(1)$$ time. 
* The `contains(int x)` method simply returns whether the `x` position in our ArrayList is `true` or `false`. This also takes $$\Theta(1)$$ time!

```text
public class DataIndexedIntegerSet {
    private boolean[] present;

    public DataIndexedIntegerSet() {
        present = new boolean[2000000000];
    }

    public void add(int x) {
        present[i] = true;
    }

    public boolean contains(int x) {
        return present[i];
    }
```

There, we have it. That's all folks.

Well, not really. What are some potential **issues** with this approach?

* Extremely wasteful. If we assume that a `boolean` takes 1 byte to store, the above needs `2GB` of space per `new DataIndexedIntegerSet()`. Moreover, the user may only insert a handful of items...
* What do we do if someone wants to insert a `String`?   
  * Let's look at this next. Of course, we may want to insert other things, like `Dog`s. That'll come soon!

