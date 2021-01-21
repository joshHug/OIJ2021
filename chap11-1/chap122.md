# 12.2 Inserting words

Our `DataIndexedIntegerSet` only allowed for integers, but now we want to insert the `String` `"cat"` into it. We'll call our data structure that can insert strings `DataIntexedEnglishWordSet` Here's a crazy idea: let's give every string a number. Maybe "cat" can be `1`, "dog" can be `2` and "turtle" can be `3`.

\(The way this would work is –– if someone wanted to add a "cat" to our data structure, we would 'figure out' that the number for "cat" is 1, and then set `present[1]` to be `true`. If someone wanted to ask us if "cat" is in our data structure, we would 'figure out' that "cat" is 1, and check if `present[1]` is true.\)

But then if someone tries to insert the word "potatocactus", we'll not know what to do. We need to develop a general strategy so that given a string, we can figure out a number representation for it.

## Strategy 1: Use the first letter.

A simple idea is to just use the first character of any given string to convert it to its number representation. So "cat" -&gt; "c" -&gt; 3. "Dog" -&gt; "d" -&gt; 4. But also, "drum" -&gt; "d" -&gt; 4.

What if someone wanted to insert "dog" and "drum" into our `DataIntexedEnglishWordSet`? All bets are off, and we don't know how to do it.

Note that when two different inputs \("dog" and "drum"\) map to the same integer, we call that a **collision**. We don't know how to deal with collisions yet, so let's figure out a way to avoid them. \(What we do with most of our problems lol...\)

## Strategy 2: Avoiding collisions

To motivate this part, let's understand how our number system works.

A four digit number, say 5149, can be written as

$$5 \cdot 10^3 + 1 \cdot 10^2 + 4 \cdot 10^1 + 9 \cdot 10^0$$.

Actually, **any** 4 digit number can be written **uniquely** in this form. What that means is given 4 digits, $$a, b, c, d$$, we can write $$a \cdot 10^3 + b \cdot 10^2 + c \cdot 10^1 + d \cdot 10^0$$ and that gives us a unique 4 digit number: $$abcd$$.

Notice that the $$10$$ is important here. If we chose a bad number, say 2, the same is not true. Let's make sure we see what happens if we chose $$2$$ as the multiplier.

$$a, b, c, d = 1, 1, 1, 1$$ gives $$1 \cdot 2^3 + 1 \cdot 2^2 + 1 \cdot 2^1 + 1 \cdot 2^0 = 15$$.  
$$a, b, c, d = 0, 3, 1, 1$$ gives $$0 \cdot 2^3 + 3 \cdot 2^2 + 1 \cdot 2^1 + 1 \cdot 2^0 = 15$$.

A collision on inputs \(1, 1, 1, 1\) and \(0, 3, 1, 1\)!

So why is $$10$$ important? It's because there are 10 unique digits in our decimal system: $$0, 1, 2, 3, 4, 5, 6, 7, 8, 9$$.

Similarly, there are $$26$$ unique characters in the english lowercase alphabet. Why not give each one a number: $$a=1, b=2, \ldots, z=26$$. Now, we can write any unique lowercase string in **base 26**. \(Note that **base 26** simply means that we will use **26** as the multiplier, much like we used **10** and **2** as examples above.\)

* "cat" = "c"  __$$26^2$$ _+ 'a'_  $$26^1$$ + 't'  __$$26^0$$\_ = $$3_26^2 + 1_26^1 + 20_26^0 = 2074$$. 

**Quick check**

* How do you represent "dog"?

**This representation gives a unique integer to every english word containing lowercase letters, much like using base 10 gives a unique representation to every number. We are guaranteed to not have collisions.**

## Our Data Structure `DataIndexedEnglishWordSet`

```text
public class DataIndexedEnglishWordSet {
    private boolean[] present;

    public DataIndexedEnglishWordSet() {
        present = new boolean[2000000000];
    }

    public void add(String s) {
        present[englishToInt(s)] = true;
    }

    public boolean contains(int i) {
        resent present[englishToInt(s)];
    }
}
```

Uses a helper method

```text
public static int letterNum(String s, int i) {
    /** Converts ith character of String to a letter number.
    * e.g. 'a' -> 1, 'b' -> 2, 'z' -> 26 */
    int ithChar = s.charAt(i)
    if ((ithChar < 'a') || (ithChar > 'z')) {
        throw new IllegalArgumentException();
    }

    return ithChar - 'a' + 1;
}

public static int englishToInt(String s) {
    int intRep = 0;
    for (int i = 0l i < s.length(); i += 1) {
        intRep = intRep * 26;
        intRep += letterNum(s, i);
    }

    return intRep;
}
```

## Where are we?

Recall, we started with wanting to

\(a\) Be better than $$\Theta(\log N)$$. We've now done this for integers and for single english words.

\(b\) Allow for non-comparable items. We haven't touched this yet, although we are getting there. So far, we've only learnt how to add integers and english words, both of which _are_ comparable, **but**, have we ever **used** the fact that they are comparable? I.e., have we ever tried to compare them \(like we did in BSTs\)? No. So we're getting there, but we haven't actually inserted anything non-comparable yet.

\(c\) We have data structures that insert integers and english words. Let's make a quick visit to inserting arbitrary `String` objects, with spaces and all that. And maybe even insert other languages and emojis!

\(d\) Further recall that our approach is still very wasteful of memory. We haven't solved that issue yet!

