# 5.3 Generics

## Creating Another Generic Class

{% embed url="https://www.youtube.com/watch?v=616v8kkelWA&list=PL8FaHk7qbOD43bx3teSwoeHto7lARC69N&index=4" caption="" %}

Now that we've created generic lists, such as `DLLists` and `ALists`, lets move on to a different data type: maps. Maps let you associate keys with values, for example, the statement "Josh's score on the exam is 0" could be stored in a Map that associates students to their exam scores. A map is the Java equivalent of a Python dictionary.

We're going to be creating the `ArrayMap` class, which implements the `Map61B` Interface, a restricted version of Java's built-in `Map` interface. `ArrayMap` will have the following methods:

```java
 put(key, value): Associate key with value.
 containsKey(key): Checks if map contains the key.
 get(key): Returns value, assuming key exists.
 keys(): Returns a list of all keys.
 size(): Returns number of keys.
```

For this exercise, we will ignore resizing. One thing to note about the `Map61B` interface \(and the Java `Map` interface in general\) is that each key can only have one value at a time. If Josh is mapped to 0, and then we say "Oh wait, there was a mistake! Josh actually got 100 on the exam," we erase the value 0 that's Josh maps to and replace it with 100.

Feel free to try building an `ArrayMap` on your own, but for reference, the full implementation is below.

```java
package Map61B;

import java.util.List;
import java.util.ArrayList;

/***
 * An array-based implementation of Map61B.
 ***/
public class ArrayMap<K, V> implements Map61B<K, V> {

    private K[] keys;
    private V[] values;
    int size;

    public ArrayMap() {
        keys = (K[]) new Object[100];
        values = (V[]) new Object[100];
        size = 0;
    }

    /**
    * Returns the index of the key, if it exists. Otherwise returns -1.
    **/
    private int keyIndex(K key) {
        for (int i = 0; i < size; i++) {
            if (keys[i].equals(key)) {
            return i;
        }
        return -1;
    }

    public boolean containsKey(K key) {
        int index = keyIndex(key);
        return index > -1;
    }

    public void put(K key, V value) {
        int index = keyIndex(key);
        if (index == -1) {
            keys[size] = key;
            values[size] = value;
            size += 1;
        } else {
            values[index] = value;
        }
    }

    public V get(K key) {
        int index = keyIndex(key);
        return values[index];
    }

    public int size() {
        return size;
    }

    public List<K> keys() {
        List<K> keyList = new ArrayList<>();
        for (int i = 0; i < keys.length; i++) {
            keyList.add(keys[i]);
        }
        return keyList;
    }
}
```

Note: the decision to name the generics `K` and `V` is arbitrary \(but meant to be intuitive\). We could have just as well replaced these generics with `Potato` and `Sauce`, or any other name. However, it's quite common to see generics in Java represented as a single uppercase letter, in this course and elsewhere.

There were a few interesting things here; looking at the top of the code, we stated `package Map61B;`. We will go over this a bit later, but for now just know that it means we are putting our `ArrayMap` class within a folder called Map61B. Additionally, we import `List` and `ArrayList` from `java.utils`.

**Exercise 5.2.1:** In our current implementation of ArrayMap, there is a bug. Can you figure out what it is?

**Answer:** In the `keys` method, the for loop should be iterating until `i == size`, not `keys.length`.

## ArrayMap and Autoboxing Puzzle

{% embed url="https://www.youtube.com/watch?v=mdu9nEmqxyE&t=208s" caption="" %}

If we write a test as shown below:

```java
@Test
public void test() { 
    ArrayMap<Integer, Integer> am = new ArrayMap<Integer, Integer>();
    am.put(2, 5);
    int expected = 5;
    assertEquals(expected, am.get(2));
}
```

You will find that we get a compile-time error!

```text
$ javac ArrayMapTest.java
ArrayMapTest.java:11: error: reference to assertEquals is ambiguous
    assertEquals(expected, am.get(2));
    ^
    both method assertEquals(long, long) in Assert and method assertEquals(Object, Object) in Assert match
```

We get this error because JUnit's `assertEquals` method is overloaded, eg. `assertEquals(int expected, int actual)`, `assertEquals(Object expected, Object actual)`, etc. Thus, Java is unsure which method to call for `assertEquals(expected, am.get(2))`, which requires one argument to be autoboxed/unboxed.

**Excercise 5.2.2** What would we need to do in order to call `assertEquals(long, long)`? A.\) Widen `expected` to a `long` B.\) Autobox `expected` to a `Long` C.\) Unbox `am.get(2)` D.\) Widen the unboxed `am.get(2)` to long

**Answer** A, C, and D all work.

**Excercise 5.2.3** How would we make it work with `assertEquals(Object, Object)`?

**Answer** Autobox `expected` to an `Integer` because `Integers` are `Objects`.

**Excercise 5.2.4** How do we make the code compile with casting?

**Answer** Cast `expected` to `Integer`.

## Generic Methods

The goal for the next section is to create a class `MapHelper` which will have two methods:

* `get(Map61B, key)`: Returns the value corresponding to the given key in the map if it exists, otherwise null.
  * This is useful because `ArrayMap` currently has a bug where the get method throws an ArrayIndexOutOfBoundsException if we try to get a key that doesn't exist in the `ArrayMap`.
* `maxKey(Map61B)`: Returns the maximum of all keys in the given `ArrayMap`. Works only if keys can be compared.

### Implementing get

`get` is a static method that takes in a Map61B instance and a key and returns the value that corresponds to the key if it exists, otherwise returns null.

**Excercise 5.2.5** Try writing this method yourself!

{% embed url="https://www.youtube.com/watch?v=TnWTIX\_mrcQ&index=6&list=PL8FaHk7qbOD43bx3teSwoeHto7lARC69N" caption="" %}

As you see in the video, we could write a very limited method by declare the parameters as String and Integer like so:

```java
public static Integer get(Map61B<String, Integer> map, String key) {
    ...
}
```

We are restricting this method to only take in `Map61B<String, Integer>`, which is not what we want! We want it to take any kind of `Map61B`, no matter what the actual types for the generics are. However, the following method header produces a compilation error:

```java
public static V get(Map61B<K, V> map, String key) {
    ...
}
```

This is because with generics defined in class headers, Java waits for the user to instantiate an object of the class in order to know what actual types each generic will be. However, here we'd like a generic specific to this method. Moreover, we do not care what actual types `K` and `V` take on in our `Map61B` argument -- the important part is that whatever `V` is, an object of type `V` is returned.

Thus we see the need for generic methods. To declare a method as generic, the formal type parameters must be specified before the return type:

```java
public static <K,V> V get(Map61B<K,V> map, K key) {
    if map.containsKey(key) {
        return map.get(key);
    }
    return null;
}
```

Here's an example of how to call it:

```java
ArrayMap<Integer, String> isMap = new ArrayMap<Integer, String>();
System.out.println(mapHelper.get(isMap, 5));
```

You don't need any explicit declaration of what type you are inserting. Java can infer that isMap is an `ArrayMap` from `Integers` to `Strings`.

## Implementing maxKey

**Exercise 5.2.6** Try writing this method yourself!

Here's something that looks OK, but isn't quite correct:

```java
public static <K, V> K maxKey(Map61B<K, V> map) {
    List<K> keylist = map.keys();
    K largest = map.get(0);
    for (K k: keylist) {
        if (k > largest) {
            largest = k;
        }
    }
    return largest;
}
```

**Exercise 5.2.7** Can you spot what's wrong with this method?

**Answer:** The `>` operator can't be used to compare `K` objects. This only works on primitives and `map` may not hold primitives

We will rewrite this method as such:

```java
public static <K, V> K maxKey(Map61B<K, V> map) {
    List<K> keylist = map.keys();
    K largest = map.get(0);
    for (K k: keylist) {
        if (k.compareTo(largest)) {
            largest = k;
        }
    }
    return largest;
}
```

**Exercise 5.2.8** This is also wrong, why?

**Answer** Not all objects have a `compareTo` method!

{% embed url="https://www.youtube.com/watch?v=W9P4feez2d0&list=PL8FaHk7qbOD43bx3teSwoeHto7lARC69N&index=7" caption="" %}

We will introduce a little more syntax for generic methods in the header of the function.

```java
public static <K extends Comparable<K>, V> K maxKey(Map61B<K, V> map) {...}
```

The `K extends Comparable<K>` means keys must implement the comparable interface and can be compared to other K's. We need to include the `<K>` after `Comparable` because `Comparable` itself is a generic interface! Therefore, we must specify what kind of comparable we want. In this case, we want to compare K's with K's.

## Type upper bounds

You might be wondering, why does it "extend" comparable and not "implement"? Comparable is an interface after all.

Well, it turns out, "extends" in this context has a different meaning than in the polymorphism context.

When we say that the Dog class extends the Animal class, we are saying that Dogs can do anything that animals can do and more! We are **giving** Dog the abilities of an animal. When we say that K extends Comparable, we are simply stating a fact. We aren't **giving** K the abilities of a Comparable, we are just saying that K **must be** Comparable. This different use of `extends` is called type upper bounding. Confusing? That's okay, it _is_ confusing. Just remember, in the context of inheritance, the `extends` keyword is active in giving the subclass the abilities of the superclass. You can think of it as a fairy Godmother: she sees your needs and helps you out with some of her fairy magic. On the other hand, in the context of generics, `extends` simply states a fact: You must be a subclass of whatever you're extending. **When used with generics \(like in generic method headers\), `extends` imposes a constraint rather than grants new abilities.** It's akin to a fortune teller, who just tells you something without doing much about it.

## Summary

We’ve seen four new features of Java that make generics more powerful:

* Autoboxing and auto-unboxing of primitive wrapper types.
* Promotion/widening between primitive types.
* Specification of generic types for methods \(before return type\).
* Type upper bounds in generic methods \(e.g. `K extends Comparable<K>`\).

