# 6.4 Object Methods

All classes inherit from the overarching Object class. The methods that are inherited are as follows:

* `String toString()`
* `boolean equals(Object obj)`
* `Class <?> getClass()`
* `int hashCode()`
* `protected Objectclone()`
* `protected void finalize()`
* `void notify()`
* `void notifyAll()`
* `void wait()`
* `void wait(long timeout)`
* `void wait(long timeout, int nanos)`

We are going to focus on the first two in this chapter. We will take advantage of inheritance to override these two methods in our classes to behave in the ways we want them to.

## toString\(\)

{% embed url="https://youtu.be/AKnMv0ootkg?list=PL8FaHk7qbOD4vPE\_Bd8QagarKi3kPw8rB" caption="" %}

The `toString()` method provides a string representation of an object. The `System.out.println()` function implicitly calls this method on whatever object is passed to it and prints the string returned. When you run `System.out.println(dog)`, it's actually doing this:

```java
String s = dog.toString()
System.out.println(s)
```

The default `Object` class' `toString()` method prints the location of the object in memory. This is a hexidecimal string. Classes like Arraylist and java arrays have their own overridden versions of the `toString()` method. This is why, when you were working with and writing tests for Arraylist, errors would always return the list in a nice format like this \(1, 2, 3, 4\) instead of returning the memory location.

For classes that we've written by ourselves like `ArrayDeque`, `LinkedListDeque`, etc, we need to provide our own `toString()` method if we want to be able to see the objects printed in a readable format.

Let's try to write this method for an `ArraySet` class. Read the `ArraySet` class below and make sure you understand what the various methods do. Feel free to plug the code into java visualizer to get a better understanding!

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

    /* Associates the specified value with the specified key in this map.
       Throws an IllegalArgumentException if the key is null. */
    public void add(T x) {
        if (x == null) {
            throw new IllegalArgumentException("can't add null");
        }
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

    /** returns an iterator (a.k.a. seer) into ME */
    public Iterator<T> iterator() {
        return new ArraySetIterator();
    }

    private class ArraySetIterator implements Iterator<T> {
        private int wizPos;

        public ArraySetIterator() {
            wizPos = 0;
        }

        public boolean hasNext() {
            return wizPos < size;
        }

        public T next() {
            T returnItem = items[wizPos];
            wizPos += 1;
            return returnItem;
        }
    }

    @Override
    public String toString() {
        /* hmmm */
    }


    @Override
    public boolean equals(Object other) {
        /* hmmm */
    }

    public static void main(String[] args) {
        ArraySet<Integer> aset = new ArraySet<>();
        aset.add(5);
        aset.add(23);
        aset.add(42);

        //iteration
        for (int i : aset) {
            System.out.println(i);
        }

        //toString
        System.out.println(aset);

        //equals
        ArraySet<Integer> aset2 = new ArraySet<>();
        aset2.add(5);
        aset2.add(23);
        aset2.add(42);

        System.out.println(aset.equals(aset2));
        System.out.println(aset.equals(null));
        System.out.println(aset.equals("fish"));
        System.out.println(aset.equals(aset));
}
```

You can find the [solutions here \(ArraySet.java\)](https://github.com/Berkeley-CS61B/lectureCode-sp19/blob/af2325c600010a8894a6ce3a3ccf517547145ec1/inheritance4/ArraySet.java)

**Exercise 6.4.1:** Write the toString\(\) method so that when we print an ArraySet, it prints the elements separated by commas inside of curly braces. i.e {1, 2, 3, 4}. Remember, the toString\(\) method should return a string.

**Solution**

```java
public String toString() {
    String returnString = "{";
    for (int i = 0; i < size; i += 1) {
        returnString += keys[i];
        returnString += ", ";
    }
    returnString += "}";
    return returnString;
}
```

This solution, although seemingly simple and elegant, is actually very naive. This is because when you use string concatenation in Java like so: `returnString += keys[i];` you are actually not just appending to `returnString`, you are creating an entirely new string. This is incredibly inefficient because creating a new string object takes time too! Specifically, linear in the length of the string.

**Bonus Question:** Let's say concatenating one character to a string takes 1 second. If we have an ArraySet of size 5: `{1, 2, 3, 4, 5}`, how long would it take to run the `toString()` method?

**Answer:** We set `returnString` to the left bracket which takes one second because this involves adding `{` to the empty string `""`. Adding the first element will involve creating an entirely new string, adding } and 1 which would take 2 seconds. Adding the second element takes 3 seconds because we need to add `{`, `1`, `2`. This process continues, so for the entire array set the total time is `1 + 2 + 3 + 4 + 5 + 6 + 7.`

To remedy this, Java has a special class called `StringBuilder`. It creates a string object that is mutable, so you can continue appending to the same string object instead of creating a new one each time.

**Exercise 6.4.2:** Rewrite the toString\(\) method using StringBuilder.

**Solution**

```java
public String toString() {
        StringBuilder returnSB = new StringBuilder("{");
        for (int i = 0; i < size - 1; i += 1) {
            returnSB.append(items[i].toString());
            returnSB.append(", ");
        }
        returnSB.append(items[size - 1]);
        returnSB.append("}");
        return returnSB.toString();
    }
```

Now you've successfully overridden the `toString()` method! Try printing the ArraySet to see the fruits of your work.

Next we will override another important object method: `equals()`

## equals\(\)

{% embed url="https://youtu.be/qHuS1o97nfQ?list=PL8FaHk7qbOD4vPE\_Bd8QagarKi3kPw8rB" caption="" %}

`equals()` and `==` have different behaviors in Java. `==` Checks if two objects are actually the same object in memory. Remember, pass-by-value! `==` checks if two boxes hold the same thing. For primitives, this means checking if the values are equal. For objects, this means checking if the address/pointer is equal.

Say we have this `Doge` class:

```java
public class Doge {

   public int age;
   public String name;

   public Doge(int age, String name){
      this.age = age;
      this.name = name;
   }
   public static void main(String[] args) {

      int x = 5;
      int y = 5;
      int z = 6;

      Doge fido = new Doge(5, "Fido");
      Doge doggo = new Doge(6, "Doggo");
      Doge fidoTwin = new Doge(5, "Fido");
      Doge fidoRealTwin = fido;
   }
}
```

If we plug this code into the java visualizer, we will see the box in pointer diagram shown below.

![](../.gitbook/assets/Doge.png)

Exercise 6.4.2: What would java return if we ran the following?

* `x == y`
* `x == z`
* `fido == doggo`
* `fido == fidoTwin`
* `fido = fidoRealTwin`

**Answers**

* `True`
* `False`
* `False`
* `False`
* `True`

`fido` and `fidoTwin` are not considered `==` because they point to different objects. However, this is quite silly since all their attributes are the same. You can see how `==` can cause some problems in Java testing. When we write tests for our ArrayList and want to check if expected is the same as what is returned by our function, we create expected as a new arraylist. If we used `==` in our test, it would always return false. This is what `equals(Object o)` is for.

## `equals(Object o)`

`equals(Object o)` is a method in the Object that, by default, acts like == in that it checks if the memory address of the this is the same as o. However, we can override it to define equality in whichever way we wish! For example, for two Arraylists to be considered equal, they just need to have the same elements in the same order.

**Exercise 6.4.3:** Let's write an equals method for the ArraySet class. Remember, a set is an unordered collection of unique elements. So, for two sets to be considered equal, you just need to check if they have the same elements.

**Solution**

```java
public boolean equals(Object other) {
        if (this == other) {
            return true;
        }
        if (other == null) {
            return false;
        }
        if (other.getClass() != this.getClass()) {
            return false;
        }
        ArraySet<T> o = (ArraySet<T>) other;
        if (o.size() != this.size()) {
            return false;
        }
        for (T item : this) {
            if (!o.contains(item)) {
                return false;
            }
        }
        return true;
    }
```

We added a few checks in the beginning of the method to make sure our equals can handle nulls and objects of a different class. We also optimized the function by return true right away if the == methods returns true. This way, we avoid the extra work of iterating through the set.

**Rules for Equals in Java:** When overriding a `.equals()` method, it may sometimes be trickier than it seems. A couple of rules to adhere to while implementing your `.equals()` method are as follows:

1.\) `equals` must be an equivalence relation

* **reflexive**: `x.equals(x)` is true
* **symmetric**: `x.equals(y)` if and only if `y.equals(x)`
* **transitive**: `x.equals(y)` and `y.equals(z)` implies `x.equals(z)`

2.\) It must take an Object argument, in order to override the original `.equals()` method

3.\) It must be consistent if `x.equals(y)`, then as long as `x` and `y` remain unchanged: `x` must continue to equal `y`

4.\) It is never true for null `x.equals(null)` must be false

## Bonus video

Create an even better `toString` method and `ArraySet.of`:

{% embed url="https://youtu.be/tjLpeVD0KWc?list=PL8FaHk7qbOD4vPE\_Bd8QagarKi3kPw8rB" caption="" %}

Link to the [bonus code](https://github.com/Berkeley-CS61B/lectureCode-sp19/blob/af2325c600010a8894a6ce3a3ccf517547145ec1/inheritance4/ArraySet.java)

