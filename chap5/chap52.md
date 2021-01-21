# 5.2 Immutability

{% embed url="https://youtu.be/Gz6LXRSjAgk" caption="" %}

The notion of immutability is one of the things you might never have known existed, but that can greatly simplify your life once you realize it's a thing \(sort of like the realization you get as an adult that nobody _really_ knows what they're doing, at least when they first start doing something new\).

An immutable data type is a data type whose instances cannot change in any observable way after instantiation.

For example, `String` objects in Java are immutable. No matter what, if you have an instance of `String`, you can call any method on that `String`, but it will remain completely unchanged. This means that when `String` objects are concatenated, neither of the original Strings are modified -- instead, a totally new `String` object is returned.

Mutable datatypes include objects like `ArrayDeque` and `Planet`. We can add or remove items from an `ArrayDeque`, which are observable changes. Similarly, the velocity and position of a `Planet` may change over time.

Any data type with non-private variables is mutable, unless those variables are declared `final` \(this is not the only condition for mutability -- there are many other ways of defining a data type so that it is mutable\). This is because an outside method can change the value of non-private variables, leading to observable change.

The `final` keyword is a keyword for variables that prevents the variable from being changed after its first assignment. For example, consider the `Date` class below:

```text
public class Date {
    public final int month;
    public final int day;
    public final int year;
    private boolean contrived = true;
    public Date(int m, int d, int y) {
        month = m; day = d; year = y;
    }
}
```

This class is immutable. After instantiating a `Date`, there is no way to change the value of any of its properties.

Advantages of immutable data types:

* Prevents bugs and makes debugging easier because properties cannot change ever
* You can count on objects to have a certain behavior/trait

Disadvantages:

* You need to create a new object in order to change a property

Caveats:

* Declaring a reference as **final** does not make the object that reference is pointing to immutable! For example, consider the following code snippet:

  ```java
         public final ArrayDeque<String>() deque = new ArrayDeque<String>();
  ```

  The `deque` variable is final and can never be reassigned, but the array deque object its pointing to can change! ArrayDeques are always mutable!

* Using the Reflection API, it is possible to make changes even to private variables! Our notion of immutability assumes that we're not using any of the special capabilities of this library.

