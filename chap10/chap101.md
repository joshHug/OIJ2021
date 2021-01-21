# 10.1 Intro to ADTs

{% embed url="https://www.youtube.com/watch?v=aFOSePlOExw&list=PL8FaHk7qbOD5JBcKwLbuUf6BAdotM4-1K" caption="" %}

An Abstract Data Type \(ADT\) is defined only by its operations, not by its implementation. For example in proj1a, we developed an ArrayDeque and a LinkedListDeque that had the same methods, but how those methods were written was very different. In this case, we say that ArrayDeque and LinkedListDeque are _implementations_ of the Deque ADT. From this description, we see that ADT's and interfaces are somewhat related. Conceptually, Deque is an interface for which ArrayDeque and LinkedListDeque are its implementations. In code, in order to express this relationship, we have the ArrayDeque and LinkedListDeque classes inherit from the Deque interface.

Some commonly used ADT's are:

* Stacks: Structures that support last-in first-out retrieval of elements
  * `push(int x)`: puts x on the top of the stack
  * `int pop()`: takes the element on the top of the stack
* **Lists**: an ordered set of elements
  * `add(int i)`: adds an element
  * `int get(int i)`: gets element at index i
* **Sets**: an unordered set of unique elements \(no repeats\)
  * `add(int i)`: adds an element
  * `contains(int i)`: returns a boolean for whether or not the set contains the value
* **Maps**: set of key/value pairs
  * `put(K key, V value)`: puts a key value pair into the map
  * `V get(K key)`: gets the value corresponding to the key

\*\*The bolded ADT's are a subinterfaces of a bigger overarching interface called `Collections`

Below we show the relationships between the interfaces and classes. Interfaces are in white, classes are in blue.

![](../.gitbook/assets/Screen%20Shot%202019-02-27%20at%201.54.27%20PM.png)

ADT's allow us to make use of object oriented programming in an efficient and elegant way. You saw in proj1b how we could swap `OffByOne` and `OffByN` comparators because they both implemented the same interface! In the same way, you can use an ArrayDeque or a LinkedListArrayDeque interchangeably because they are both part of the Deque ADT.

In the following chapters, we will work on defining some more ADT's and enumerating their different implementations.

