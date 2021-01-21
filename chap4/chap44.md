# 4.4 Java libraries and packages

## Abstract Data Types \(ADTS\)

{% embed url="https://www.youtube.com/watch?v=XZnZkASrArc&list=PL8FaHk7qbOD56QlcKsw4AfqJkkO5IMiyG" caption="" %}

Despite not talking about them explicitly, we have actually seen a few Abstract Data types already in class!

Two examples are List61B and Deque. Let's hone in on Deque.

![deque](../.gitbook/assets/deque.png)

We have this interface `deque` that both ArrayDeque and LinkedListDeque implement. What is the relationship between Deque and its implementing classes? Well, deque simply provides a list of methods \(behaviors\):

```java
public void addFirst(T item);
public void addLast(T item);
public boolean isEmpty();
public int size();
public void printDeque();
public T removeFirst();
public T removeLast();
public T get(int index);
```

These methods are actually **implemented** by ArrayDeque and LinkedListDeque.

In Java, Deque is called an interface. Conceptually, we call deque an **Abstract data type**. Deque only comes with behaviors, not any concrete ways to exhibit those behaviors. In this way, it is abstract.

## Java Libraries

Java has certain built-in Abstract data types that you can use. These are packaged in Java Libraries.

The three most important ADTs come in the java.util library:

* [List](https://docs.oracle.com/javase/8/docs/api/java/util/List.html): an ordered collection of items
  * A popular implementation is the [ArrayList](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html)
* [Set](https://docs.oracle.com/javase/7/docs/api/java/util/Set.html): an unordered collection of strictly unique items \(no repeats\)
  * A popular implementation is the [HashSet](https://docs.oracle.com/javase/7/docs/api/java/util/HashSet.html)
* [Map](https://docs.oracle.com/javase/8/docs/api/java/util/Map.html): a collection of key/value pairs. You access the value via the key.
  * A popular implementation is the [HashMap](https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html)

Finish the exercises below by using the above three ADT's. Reading the documentations linked above will help immensely.

**Exercise 4.4.1** Write a method `getWords` that takes in a `String inputFileName` and puts every word from the input file into a list. Recall how we read words from a file in proj0. _\*Hint_: use `In`

**Exercise 4.4.2** Write a method `countUniqueWords` that takes in a `List<String>` and counts how many **unique** words there are in the file.

{% embed url="https://www.youtube.com/watch?v=SViq7Puj4\_k&index=2&list=PL8FaHk7qbOD56QlcKsw4AfqJkkO5IMiyG" caption="" %}

We used a list for the first exercise and set for the second.

```java
public static List<String> getWords(String inputFileName) {
    List<String> lst = new ArrayList<String>();
    In in = new In();
    while (!in.isEmpty()) {
        lst.add(in.readString()); //optionally, define a cleanString() method that cleans the string first.       
    }
    return lst;
}

public static int countUniqueWords(List<String> words) {
    Set<String> ss = new HashSet<>();
    for (String s : words) {
           ss.add(s);        
    }
    return ss.size();
}
```

**Exercise 4.4.3** Write a method `collectWordCount` that takes in a `List<String> targets` and a `List<String> words` and finds the number of times each target word appears in the word list.

{% embed url="https://www.youtube.com/watch?v=0JChT2oaC5M&list=PL8FaHk7qbOD56QlcKsw4AfqJkkO5IMiyG&index=3" caption="" %}

```java
public static Map<String, Integer> collectWordCount(List<String> words) {
    Map<String, Integer> counts = new HashMap<String, Integer>();
    for (String t: target) {
        counts.put(s, 0);
    }
    for (String s: words) {
        if (counts.containsKey(s)) {
            counts.put(word, counts.get(s)+1);
        }
    }
    return counts;
}
```

We used a map because it makes an association between two things. In our case, we need an association between word and number.

These three ADT's all extend from the Collection Interface. The collection interface is super vague. Java says collections "represent a group of objects, known as its elements".

![h](../.gitbook/assets/collection_hierarchy.png)

In the diagram above, the white boxes are interfaces. The blue boxes are concrete classes.

## Java vs Python

{% embed url="https://www.youtube.com/watch?v=rWZInNwe3\_U&list=PL8FaHk7qbOD56QlcKsw4AfqJkkO5IMiyG&index=4" caption="" %}

Java is pretty verbose. The java code below looks a lot more cumbersome than the corresponding python code.

![hi](../.gitbook/assets/java.png)

![hbai](../.gitbook/assets/python.png)

But, Java has its upsides too! It gives you a lot of choices and freedom. For example, python only has one dictionary type which is declared using curly brackets {}. With Java, if you want to use an ADT like a Map, you can choose what kind of map you want: a Hashmap? a Treemap? etc.

We like Java in 61B! Here are some reasons why:

* Arguably, takes less time to write programs, due to features like:
  * Static types \(provides type checking and helps guide programmer\).
  * Bias towards interface inheritance leading to cleaner subtype polymorphism.
  * Access control modifiers make abstraction barriers more solid.
* More efficient code, due to features like:
  * Ability to have more control over engineering tradeoffs.
  * Single valued arrays lead to better performance.
* Basic data structures more closely resemble underlying hardware:
  * Would be weird to do ArrayDeque in Python, since there is no need for array resizing. However, in hardware \(see 61C\), variable length arrays don’t exist.

## Abstract classes

{% embed url="https://www.youtube.com/watch?v=ygwF3UsMejM&index=5&list=PL8FaHk7qbOD56QlcKsw4AfqJkkO5IMiyG" caption="" %}

We've seen interfaces that can do a lot of cool things! They allow you to take advantage of interface inheritance and implementation inheritance. As a refresher, these are the qualities of interfaces:

* All methods must be public.
* All variables must be public static final.
* Cannot be instantiated
* All methods are by default abstract unless specified to be `default`
* Can implement more than one interface per class

We will now introduce a new class that lies somewhere in between interfaces and concrete classes: the abstract class. Below are the characteristics of abstract classes:

* Methods can be public or private
* Can have any types of variables
* Cannot be instantiated
* Methods are by default concrete unless specified to be `abstract`
* Can only implement one per class

Basically, abstract classes can do everything interfaces can do and more.

**When in doubt, try to use interfaces** in order to reduce complexity.

## Packages

{% embed url="https://www.youtube.com/watch?v=xQuYmp9dE2U&list=PL8FaHk7qbOD56QlcKsw4AfqJkkO5IMiyG&index=6" caption="" %}

Package names give **give a canonical name for everything**. Canonical means a _unique representation_ for a thing.

Why? Well, in Java, we could have multiple classes with the same name. We need a way to differentiate between these different classes. In industry, this differentiation happens by appending the class to a website address \(backwards\) like below:

```text
But... this means we have to type out that entire name every time we want to instantiate something of that class.

```ug.joshh.animal.Dog d = new ug.joshh.animal.Dog()
```

This is annoying. We can remedy this by importing the package.

`import ug.joshh.animal`

Now we can use dogs as we please.

This is just a brief preview of packages. We will get to more of this during later weeks of the course.

