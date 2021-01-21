# 7.2 Access Control

## Access Control

{% embed url="https://www.youtube.com/watch?v=xxngey7AFYw" caption="" %}

We now run into the question of how public and private members behave in packages and subclasses. Think to yourself right now: when inheriting from a parent class, can we access the private members in that parent class? Or, can two classes in the same package access the other’s private members?

If you don’t know the answers right away, you can read on to find out!

**Private** Only code from the given class can access **private** members. It is truly _private_ from everything else, as subclasses, packages, and other external classes cannot access private members. _TL;DR: only the class needs this piece of code_

**Package Private** This is the default access given to Java members if there is no explicit modifier written. Package private entails that classes that belong in the same package can access, but not subclasses! Why is this useful? Usually, packages are handled and modified by the same \(group of\) people. It is also common for people to extend classes that they didn’t initially write. The original owners of the class that’s being extended may not want certain features or members to be tampered with, if people choose to extend it — hence, package-private allows those who are familiar with the inner workings of the program to access and modify certain members, whereas it blocks those who are subclassing from doing the same. _TL;DR: only classes that live in the same package can access_

**Protected** Protected members are protected from the “outside” world, so classes within the same package and subclasses can access these members, but the rest of the world \(e.g. classes external to the package or non-subclasses\) cannot! _TL;DR: subtypes might need it, but subtype clients will not_

**Public** This keyword opens up the access to everyone! This is generally what clients of the package can rely on to use, and once deployed, the public members’ signatures should not change. It’s like a promise and contract to people using this public code that it will always be accessible to them. Usually if developers want to “get rid of” something that’s public, rather than removing it, they would call it “deprecated” instead.

_TL;DR: open and promised to the world_

**Exercise 7.1.1** See if you can draw the access table yourself, from memory.

Have the following be the column titles: Modifier, Class, Package, Subclass, World, with the following as the Rows: public, protected, package-private, private.

Indicate whether or not each row/access type has access to that particular column’s “type”.

![access](../.gitbook/assets/access_modifiers.png)

## Access Control Subtleties

{% embed url="https://www.youtube.com/watch?v=0tjxUDoIAhw" caption="" %}

**Default Package** Code that does not have a package declaration is automatically part of the default package. If these classes have members that don’t have access modifiers \(i.e. are package-private\), then because everything is part of the same \(unnamed\) default package, these members are still accessible between these “default”-package classes.

**Access is Based Only on Static Types** It is important to note that for interfaces, the default access for its methods is actually public, and not package-private. Additionally, like this subtitle indicates, the access depends only on the static types.

**Exercise 7.1.2**

Given the following code, which lines in the demoAccess method, if any, will error during compile time?

```java
package universe;
public interface BlackHole {
    void add(Object x); // this method is public, not package-private!
}

package universe;
public class CreationUtils {
    public static BlackHole hirsute() {
         return new HasHair();
    }
}

package universe;
class HasHair implements BlackHole {
    Object[] items;
    public void add(Object o) { ... }
    public Object get(int k) { ... }
}

import static CreationUtils.hirsute;
class Client {
   void demoAccess() {
      BlackHole b = hirsute();
      b.add("horse");
      b.get(0);
      HasHair hb = (HasHair) b;
   }
}
```

**ANSWER**

* b.get\(0\); This line errors because `b` is of static type `BlackHole`, but the `BlackHole` interface does not define a `get` method! Even though you and I both know that `b` is dynamically a `HasHair`, and thus has the `get` method, the compiler bases its checks off the static type.
* HasHair hb = \(HasHair\) b; This one is tricky, but notice that the `HasHair` class is not a public class - it's package-private. This means that `Client`, a class outside of the `universe` package, can't see that the `HasHair` class exists.

