# 7.1 Packages

{% embed url="https://www.youtube.com/watch?v=3brypGcpctQ" caption="" %}

It is very possible that with all the code in this world, you would create classes that share names with those from a different project. How can you then organize these classes, such that there is less ambiguity when you’re trying to access or use them? How will your program know that you mean to use your Dog.class, versus Josh Hug’s Dog.class?

Herein enters the **package** — a namespace that organizes classes and interfaces. In general, when creating packages you should follow the following naming convention: package name starts with the website address, backwards.

For example, if Josh Hug were trying to distribute his Animal package, which contains various different types of animal classes, he would name his package as following:

`ug.joshh.animal; // note: his website is joshh.ug`

However, in CS61B you do not have to follow this convention, as your code isn’t intended for distribution.

## Using Packages

If you’re accessing the class from within the same package, you can just use its simple name:

```java
Dog d = new Dog(...);
```

If you’re accessing the classes from outside the package, then use its entire canonical name:

```java
ug.joshh.animal.Dog d = new ug.joshh.animal.Dog(...);
```

To make things easier, you can import the package, and use the simple name instead!

```java
import ug.joshh.animal.Dog;
...
Dog d = new Dog(...);
```

## Creating a Package

Creating a package takes the following two steps:

1.\) Put the package name at the top of every file in this package

```java
package ug.joshh.animal;

public class Dog {
    private String name;
    private String breed;
    …
}
```

2.\) Store the file in a folder that has the appropriate folder name. The folder should have a name that matches your package:

i.e. `ug.joshh.animal` package is in ug/joshh/animal folder

**Creating a Package, in IntelliJ**

1.\) File → New Package

1.\) Choose package name \(i.e. “ug.joshh.animal”\)

**Adding \(new\) Java Files to a Package, in IntelliJ**

1.\) Right-click package name

2.\) Select New → Java Class

3.\) Name your class, and IntelliJ will automatically put it in the correct folder + add the “package ug.joshh.animal” declaration for you.

**Adding \(old\) Java Files to a Package, in IntelliJ**

1.\) Add “package \[packagename\]” to the top of the file.

2.\) Move the .java file into the corresponding folder.

## Default packages

Any Java class without an explicit package name at the top of the file is automatically considered to be part of the “default” package. However, when writing real programs, you should avoid leaving your files in the default package \(unless it’s a very small example program\). This is because code from the default package cannot be imported, and it is possible to accidentally create classes with the same name under the default package.

For example, if I were to create a “DogLauncher.java” class in the default package, I would be unable to access this DogLauncher class anywhere else outside of the default package.

```java
DogLauncher.launch(); // won’t work
default.DogLauncher.launch(); // doesn’t exist
```

Therefore, your Java files should generally start with an explicit package declaration.

## JAR Files

{% embed url="https://www.youtube.com/watch?v=nxE8-Q6joDU" caption="" %}

Oftentimes, programs will contain multiple .class files. If you wanted to share this program, rather than sharing all the .class files in special directories, you can “zip” all the files together by creating a JAR file. This single .jar file will contain all your .class files, along with some other additional information.

It is important to note that JAR files are just like zip files. It is entirely possible to unzip and transform the files back into .java files. JAR files do not keep your code safe, and thus you should not share your .jar files of your projects with other students.

**Creating a JAR File \(IntelliJ\)**

1.\) Go to File → Project Structure → Artifacts → JAR → “From modules with dependencies”

2.\) Click OK a couple of times

3.\) Click Build → Build Artifacts \(this will create a JAR file in a folder called “Artifacts”\)

4.\) Distribute this JAR file to other Java programmers, who can now import it into IntelliJ \(or otherwise\)

**Build Systems**

Rather than importing a list of libraries or whatnot each time we wanted to create a project, we can simply put the files into the appropriate place, and use “Build Systems” to automate the process of setting up your project. The advantages of Build Systems are especially seen in bigger teams and projects, where it’s largely beneficial to automate the process of setting up the project structure. Though the advantages of Build Systems are rather minimal in 61B, we did use Maven in Project 3 \(BearMaps, Spring 2017\), which is one of many popular build systems \(including Ant and Gradle\).

