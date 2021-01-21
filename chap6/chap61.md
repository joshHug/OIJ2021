# 6.1 Lists, Sets, and ArraySet

In this section we will learn about how to use Java's built in `List` and `Set` data structures as well as build our own `ArraySet`.

{% embed url="https://youtu.be/DWr8YNXPH6k?list=PL8FaHk7qbOD4vPE\_Bd8QagarKi3kPw8rB" caption="" %}

In this course, we've already built two kinds of lists: `AList` and `SLList`. We also built an interface `List61B` to enforce specific list methods `AList` and `SLList` had to implement. You can find the code at the following links:

* [List61B](https://github.com/Berkeley-CS61B/lectureCode-sp19/blob/master/inheritance2/List61B.java)
* [AList](https://github.com/Berkeley-CS61B/lectureCode-sp19/blob/master/inheritance1/AList.java)
* [SLList](https://github.com/Berkeley-CS61B/lectureCode-sp19/blob/master/inheritance2/SLList.java)

This is how we might use `List61B` type:

```java
List61B<Integer> L = new AList<>();
L.addLast(5);
L.addLast(10);
L.addLast(15);
L.print();
```

## Lists in Real Java Code

We built a list from scratch, but Java provides a built-in `List` interface and several implementations, e.g. `ArrayList`. Remember, since `List` is an interface we can't instatiate it! We must instatiate one of its implementations.

To access this, we can use the full name \('canonical name'\) of classes, interfaces:

```java
java.util.List<Integer> L = new java.util.ArrayList<>();
```

However this is a bit verbose. In a similar way to how we import `JUnit`, we can import java libraries:

```java
import java.util.List;
import java.util.ArrayList;

public class Example {
    public static void main(String[] args) {
        List<Integer> L = new ArrayList<>();
        L.add(5);
        L.add(10);
        System.out.println(L);
    }
}
```

## Sets

Sets are a collection of unique elements - you can only have one copy of each element. There is also no sense of order.

### Java

Java has the `Set` interface along with implementations, e.g. `HashSet`. Remember to import them if you don't want to use the full name!

```java
import java.util.Set;
import java.util.HashSet;
```

Example use:

```java
Set<String> s = new HashSet<>();
s.add("Tokyo");
s.add("Lagos");
System.out.println(S.contains("Tokyo")); // true
```

### Python

In python, we simply call `set()`. To check for `contains` we don't use a method but the keyword `in`.

```python
s = set()
s.add("Tokyo")
s.add("Lagos")
print("Tokyo" in s) // True
```

## ArraySet

Our goal is to make our own set, `ArraySet`, with the following methods:

* `add(value)`: add the value to the set if not already present
* `contains(value)`: check to see if ArraySet contains the key
* `size()`: return number of values

If you would like to try it yourself, find 'Do It Yourself' [starter code here](https://github.com/Berkeley-CS61B/lectureCode-sp19/blob/af2325c600010a8894a6ce3a3ccf517547145ec1/exercises/DIY/inheritance4/ArraySet.java). In the lecture clip below, Professor Hug goes develops the solution:

Here is our code as of now:

```java
import java.util.Iterator;

public class ArraySet<T> implements Iterable<T> {
    private T[] items;
    private int size; // the next item to be added will be at position size

    public ArraySet() {
        items = (T[]) new Object[100];
        size = 0;
    }

    /* Returns true if this map contains a mapping for the specified key.
     */
    public boolean contains(T x) {
        for (int i = 0; i < size; i += 1) {
            if (items[i].equals(x)) {
                return true;
            }
        }
        return false;
    }

    /* Associates the specified value with the specified key in this map. */
    public void add(T x) {
        if (contains(x)) {
            return;
        }
        items[size] = x;
        size += 1;
    }

    /* Returns the number of key-value mappings in this map. */
    public int size() {
        return size;
    }
}
```

