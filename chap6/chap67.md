# Iteration \(legacy\)

## Iteration _\(legacy\)_

{% embed url="https://www.youtube.com/watch?v=2QvQT2ptkBo" caption="" %}

We saw that Java allows us to iterate through Lists using a convenient shorthand syntax sometimes called the “foreach” or “enhanced for” loop.

For example,

```java
List<Integer> friends =
new ArrayList<Integer>();
friends.add(5);
friends.add(23);
friends.add(42);
for (int x : friends) {
    System.out.println(x);
}
```

Let’s strip away the magic so we can build our own classes that support this.

The key here is an object called an _iterator_.

For our example, in List.java we might define an `iterator()` method that returns an iterator object.

```java
public Iterator<E> iterator();
```

Now, we can use that object to loop through all the entries in our list:

```java
List<Integer> friends = new ArrayList<Integer>();
...
Iterator<Integer> seer = friends.iterator();

while (seer.hasNext()) {
    System.out.println(seer.next());
}
```

This code behaves identically to the foreach loop version above.

There are three key methods in our iterator approach:

First, we get a new iterator object with `Iterator<Integer> seer = friends.iterator();`

Next, we loop through the list with our while loop. We check that there are still items left with `seer.hasNext()`, which will return true if there are unseen items remaining, and false if all items have been processed.

Last, `seer.next()` does two things at once. It returns the next element of the list, and here we print it out. It also advances the iterator by one item. In this way, the iterator will only inspect each item once.

## Implementing Iterators

{% embed url="https://www.youtube.com/watch?v=pqdq6SFGkGg" caption="" %}

In this section, we are going to talk about how to build a class to support iteration.

Let's start by thinking about what the compiler need to know in order to successfully compile the following iterator example:

```java
List<Integer> friends = new ArrayList<Integer>();
Iterator<Integer> seer = friends.iterator();

while(seer.hasNext()) {
    System.out.println(seer.next());
}
```

We can look at the static types of each object that calls a relevant method. `friends` is a List, on which `iterator()` is called, so we must ask:

* Does the List interface have an iterator\(\) method?

`seer` is an Iterator, on which `hasNext()` and `next()` are called, so we must ask:

* Does the Iterator interface have next/hasNext\(\) methods?

So how do we implement these requirements?

The List interface extends the Iterable interface, inheriting the abstract iterator\(\) method. \(Actually, List extends Collection which extends Iterable, but it's easier to codethink of this way to start.\)

```java
public interface Iterable<T> {
    Iterator<T> iterator();
}
```

```java
public interface List<T> extends Iterable<T>{
...
}
```

Next, the compiler checks that Iterators have `hasNext()` and `next()`. The Iterator interface specifies these abstract methods explicitly:

```java
package java.util;
public interface Iterator<T> {
    boolean hasNext();
    T next();
}
```

Specific classes will implement their own iteration behaviors for the interface methods. Let's look at an example. \(Note: if you want to build this up from the start, follow along with the live coding in the video.\)

We are going to add iteration through keys to our ArrayMap class. First, we write a new class called KeyIterator, nested inside of ArrayMap:

```java
public class KeyIterator {
    private int ptr;
    public KeyIterator() {
        ptr = 0;
    }

    public boolean hasNext() {
        return (ptr != size);
    }

    public K next() {
        K returnItem = keys[ptr];
        ptr = ptr + 1;
        return returnItem;
    }
}
```

This KeyIterator implements a `hasNext()` method, and a `next()` method, using a `ptr` variable to keep track of its position in the array of keys. For a different data structure, we might implement these two methods differently.

**Thought Excercise:** How would you design `hasNext()` and `next()` for a linked list?

Now that we have the appropriate methods, we can use a KeyIterator to iterate through an ArrayMap:

```java
ArrayMap<String, Integer> am = new ArrayMap<String, Integer>();
am.put("hello", 5);
am.put("syrups", 10);
ArrayMap.KeyIterator ami = am.new KeyIterator();

while (ami.hasNext()) {
    System.out.println(ami.next());
}
```

There's an interesting line in the code above: `am.new KeyIterator();`. This construction allows us to instantiate a non-static nested class, meaning a class that needs to be associated with a particular instance of the enclosing class. It wouldn't make sense to have a KeyIterator not associated with a particular ArrayMap -- what would it iterate through? So, we must use dot notation with a specific ArrayMap instance to create a new KeyIterator associated with that instance.

Now we have a KeyIterator, and it can loop through an ArrayMap. We still want to be able to support the enhanced for loop, though, to make our calls cleaner. So, we need to make ArrayMap implement the Iterable interface. The essential method of the Iterable interface is `iterator()`, which returns an Iterator object for that class:

```java
public class ArrayMap<K, V> implements Iterable<K> 
    @Override
    public Iterator<T> iterator() { 
        return new KeyIterator();
    }
}
```

We override that `iterator()` method to return the KeyIterator that we just wrote.

There's one more step before the code will compile: we have to tell Java that KeyIterator is an Iterator. To make that happen, KeyIterator must implement Iterator. This way, we can return a KeyIterator in our `iterator()` method above, and successfully implement the Iterator methods:

```java
public class KeyIterator implements Iterator<K> {
    private int ptr;
    public KeyIterator() { ptr = 0; }
    public boolean hasNext() { return (ptr != size); } 
    public K next() { ... }
}
```

Now we can use our enhanced for loop construction with an ArrayMap:

```java
ArrayMap<String, Integer> am = new ArrayMap<String, Integer>();
for (String s : am) {
    System.out.println(s);
}
```

Here we've seen **Iterable**, the interface that makes a class able to be iterated on, and requires the method `iterator()`, which returns an Iterator object. And we've seen **Iterator**, the interface that defines the object with methods to actually do that iteration. You can think of an Iterator as a machine that you put onto an iterable that facilitates the iteration. Any iterable is the object on which the iterator is performing.

With these two components, you can make fancy for loops for your classes!

