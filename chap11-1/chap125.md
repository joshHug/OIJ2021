# 12.5 Hash Table and Fixing Runtime

### Our Final Data Structure: `HashTable`

What we've created now is called a `HashTable`.

* Inputs are converted by a hash function \(`hashcode`\) into an integer. Then, they're converted to a valid index using the modulus operator. Then, they're added at that index \(dealing with collisions using LinkedLists\). 
* `contains` works in a similar fashion by figuring out the valid index, and looking in the corresponding LinkedList for the item. 

### Dealing with Runtime

The only issue left to solve is the issue of runtime. If we have 100 items, and our ArrayList is of size 5, then

* In the best case, all items get sent to the different indices evenly. That is, we have 5 linkedLists, and each one contains 20 of the items. 
* In the worst case, all items get sent to the same index! That is, we have just 1 LinkedList, and it has all 100 items. 

There are two ways to try to fix this:

* Dynamically growing our hashtable. 
* Improving our Hashcodes

#### Dynamically growing the hash table

Suppose we have $$M$$ buckets \(indices\) and $$N$$ items. We say that our **load factor** is $$N/M$$.

\(Note that the **load factor** is equivalent to our **best** case runtime from above.\)

So... we have incentive to keep our load factor low \(after all, it is the best runtime we could possible achieve!\).

And note that if we keep $$M$$ \(the number of buckets\) fixed, and $$N$$ keeps increasing, the load factor consistently keeps increasing.

Strategy? Every so often, just double $$M$$. The way we do this is as follows:

* Create a new HashTable with 2M buckets. 
* Iterate through all the items in the old HashTable, and one by one, add them into this new HashTable.
  * We need to add elements one by one again because since the size of the array changes, the modulus also changes, therefore the item probably belongs in a different bucket in the new hashtable than in the old one.

We do this by setting a **load factor threshold**. As soon as the load factor becomes bigger than this threshold, we resize.

Take a look at the example below. The hashcode for the "helmet" is 13. In the first hashtable, it gets sent to bucket $$13\%4 = 1$$. In the second hash table, it gets sent to bucket $$13\%8 = 5$$. **Note that resizing the hash table also helps with shuffling the items in the hashtable**!  
![](../.gitbook/assets/Screen%20Shot%202019-03-08%20at%201.37.16%20PM.png)

At this point, _assuming items are evenly distributed_, all the lists will be approximately $$N/M$$ items long, resulting in $$\Theta(N/M)$$ runtime. Remember that $$N/M$$ is only allowed to be under a constant **load factor threshold**, and so, $$\Theta(N/M) = \Theta(1)$$.

Note also that resizing takes $$\Theta(N)$$ time. Why? Because we need to add $$N$$ items to the hashtable, and each add takes $$\Theta(1)$$ time.

A small point: when doing the resize, we don't actually need to check if the items are already present in the LinkedList or not \(because we know there are no duplicates\), so we can just add each item in $$\Theta(1)$$ time for sure by adding it to the front of the linked list. \(Recall that usually we have to search the LinkedList to make sure the item isn't there... but we can skip that step when resizing.\)

Of course, we need to revisit our assumption of assuming items are evenly distributed. If items are not evenly distributed, our runtime will be $$\Theta(N)$$ because there could be a single linked list of size $$N$$.

#### Assuming that items are evenly distributed?

Items will distribute evenly if we have good hash codes \(i.e. hashcodes which give fairly random values for different items.\) Doing this in general is.. well... hard.

![](../.gitbook/assets/Screen%20Shot%202019-03-08%20at%201.44.43%20PM.png)

Some general good rules of thumb:

* Use a 'base' strategy similar to the one we developed earlier. 
* Use a 'base' that's a small prime.
  * Base 126 isn't actually very good, because using base 126 means that any string that ends in the same last 32 characters has the same hashcode. 
  * This happens because of overflow. 
  * Using prime numbers helps avoid overflow issues \(i.e., collisions due to overflow\).
  * Why a small prime? Because it's easier to compute. 

Some examples

![](../.gitbook/assets/Screen%20Shot%202019-03-08%20at%201.49.51%20PM.png)

![](../.gitbook/assets/Screen%20Shot%202019-03-08%20at%201.49.56%20PM.png)

## Next Steps

Wow, we've just gone through the creation of a data structure from scratch! Proud of you guys. To apply your knowledge finish HW3: [https://sp19.datastructur.es/materials/hw/hw3/hw3\](https://sp19.datastructur.es/materials/hw/hw3/hw3\)

