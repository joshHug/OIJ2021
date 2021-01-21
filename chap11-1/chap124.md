# 12.4 Handling Collisions

Time to address the elephant in the room. The big idea is to change our array ever so slightly to not contain just items, but instead contain a LinkedList \(or any other List\) of items. So...

Everything in the array is originally empty.  
If we get a new item, and its hashcode is $h$:

* If there is nothing at index $h$ at the moment, we'll create a new `LinkedList` for index $h$, place it there, and then add the new item to the newly created `LinkedList`. 
* If there is already something at index $h$, then there is already a `LinkedList` there. We simply add our new item to that `LinkedList`. **Note: Our data structure is not allowed to have any duplicate items / keys. Therefore, we must first check whether the item we are trying to insert is already in this LinkedList. If it is, we do nothing! This also means that we will insert to the END of the linked list, since we need to check all of the elements anyways.**

## Concrete workflow

* `add` item
  * Get hashcode \(i.e., index\) of item.
  * If index has no item, create new List, and place item there.
  * If index has a List already, check the List to see if item is already in there. If _not_, add item to List. 
* `contains` item
  * Get hashcode \(i.e., index\) of item. 
  * If index is empty, return `false`. 
  * Otherwise, check all items in the List at that index, and if the item exists, return `true`. 

## Runtime Complexity

![](../.gitbook/assets/Screen%20Shot%202019-03-08%20at%201.19.34%20PM.png)

**Why is contains** $$\Theta(Q)$$?  
Because we need to look at all the items in the LinkedList at the hashcode \(i.e., index\).

**Why is add** $$\Theta(Q)$$?  
Can't we just add to the beginning of the LinkedList, which takes $$\Theta(1)$$ time? No! Because **we have to check to make sure the item isn't already in the linked list.**

## You gain some, you lose some.

* Space: Still unsolved. 
* Handling collisions: done. 
* Runtime complexity? We've lost some. In the worst case, all of our items' `hashcode` could be the same, and so they all go to the same index. If we have $$N$$ items, it's possible that they **all** go to the same index, creating a linked list of length $$N$$, providing a runtime of $$\Theta(N)$$. 

## Solving space

Why keep an ArrayList of size 4 billion around? Recall that we did that to avoid collisions, because we wanted to be able to add every integer / word / `String` to our data structure. But now that we allow for collisions _anyway_, we can relax this a bit.

An idea: modulo. Let's just create an ArrayList of size, say, 100. Let's not change how the `hashcode` functions behaves \(let it return a crazy large integer.\) But after we get the `hashcode`, we'll take its modulo 100 to get an index within the $$0\ldots99$$ range that we want. And if collisions happen? Doesn't matter, we know how to deal with it!

Do note that our LinkedLists within the array will now be longer, because we're taking all the items spread across the 4 billions indices and compressing them into a 100 indices.

## Where we are

* Space: Has been solved.
* Handling collisions: Done!
* Runtime complexity? We lost some earlier at $$\Theta(Q)$$ for `add` and `contains`, and then in the `Solving space` section, we realized that we lost some more because our LinkedLists will potentially be larger \(so `Q` will be larger.\) 

