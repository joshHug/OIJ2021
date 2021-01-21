# 16.1 Uniform Partitioning

## Motivation

{% embed url="https://youtu.be/BV9Yi7eAEyY" caption="" %}

Suppose we want to perform operations on a set of Body objects in space. For example, perhaps we wanted to ask questions about the Sun bodies \(shown as yellow dots below\) in our two-dimension image space.

![](../.gitbook/assets/Screen%20Shot%202019-03-15%20at%2010.46.02%20AM.png)

### First Question: 2D Range Finding

One question we might ask is: **How many objects are in a region**, such as in the highlighted green rectangle above?

### Second Question: Nearest Neighbors

Another question we might ask is: **What is the closest object to another object**, such as which sun is closest to our space horse? \(The desired answer as found by visual inspection is the sun closest to its back hoof.\)

## Initial Attempt: HashTable

**Question**: If our set of suns were stored in a HashTable, what is the runtime for finding the answer to our Nearest Neighbors question?

**Solution**: The bucket that each object resides in is effectively random, and so we would have to iterate over all $$N$$ items to check if each sun could possibly be the closest to the horse. $$\Theta(N)$$.

Let's try to improve so that we don't have to look at every single sun in our set to find our answer.

## Second Attempt: Uniform Partitioning

{% embed url="https://youtu.be/Ua7vmGcY3Qg" caption="" %}

The problem with hash tables is that the bucket number of an item is effectively random. Hash tables are, by definitely, unordered collections. One fix is to ensure that the bucket numbers depend only on position!

If we uniformly partition our image space by throwing a 4x4 grid over it, we get nice organized buckets that look something like this \(this is also sometimes called "spatial hashing"\):

![](../.gitbook/assets/Screen%20Shot%202019-03-15%20at%2011.11.28%20AM.png)

This can be implemented by not using the object's `hashCode()` function, and instead having each object provide a `getX()` and `getY()` function so that it can compute its own bucket number.

Now, we know which grid cells our searches can be confined to, and we only have to look at suns in those particular cells rather than looking at all the suns in our entire image space as we had to before.

**How many objects are in a region?**: We jsut need to look in buckets 5, 6, 9, and 10.

**Which sun is closest to the horse?**: First, we start in the cell that the horse resides in: 1. Then, we can move outwards to 0, 4, 5, 6, and 2. etc.

**Question**: Using uniform partitioning, what is the runtime for finding the answer to our Nearest Neighbors question, assuming the suns are evenly spread out?

**Solution**: On average, the runtime will be $$16$$ times faster than without spatial partitioning, but unfortunately $$N \over 16$$ is still $$\Theta (N)$$. BUT, this does indeed work better in practice.

Still, let's see if there's an even better way.

