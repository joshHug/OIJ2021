# 8.2 Asymptotics I

{% embed url="https://www.youtube.com/watch?v=DF1ThvyLwnk&list=PL8FaHk7qbOD69aH2dhqcY64VMmX7frTUO&index=1" caption="" %}

We can consider the process of writing efficient programs from two different perspectives:

1. Programming Cost _\(everything in the course up to this date\)_
   1. How long does it take for you to develop your programs?
   2. How easy is it to read or modify your code?
   3. How maintainable is your code? \(very important — much of the cost comes from maintenance and scalability, not development!\)
2. Execution Cost _\(everything in the course from this point on\)_
   1. **Time complexity**: How much time does it take for your program to execute?
   2. **Space complexity**: How much memory does your program require?

## Example of Algorithm Cost

Objective: Determine if a _sorted_ array contains any duplicates.

**Silly Algorithm**: Consider _**every**_ pair, returning true if any match!

**Better Algorithm:** Take advantage of the _**sorted**_ nature of our array.

* We know that if there are duplicates, they must be next to each other.
* Compare neighbors: return true first time you see a match! If no more items, return false.

We can see that the Silly algorithm seems like it’s doing a lot more unnecessary, redundant work than the Better algorithm. But how much more work? How do we actually quantify or determine how efficient a program is? This chapter will provide you the formal techniques and tools to compare the efficiency of various algorithms!

## Runtime Characterization

{% embed url="https://www.youtube.com/watch?v=mRn8Z46psX8&list=PL8FaHk7qbOD69aH2dhqcY64VMmX7frTUO&index=2\)" caption="" %}

To investigate these techniques, we will be characterizing the runtimes of the following two functions, dup1 and dup2. These are the two different ways of finding duplicates we discussed above.

Things to keep in mind about our characterizations:

* They should be simple and mathematically rigorous.
* They should also clearly demonstrate the superiority of dup2 over dup1.

```java
//Silly Duplicate: compare everything
public static boolean dup1(int[] A) {  
  for (int i = 0; i < A.length; i += 1) {
    for (int j = i + 1; j < A.length; j += 1) {
      if (A[i] == A[j]) {
         return true;
      }
    }
  }
  return false;
}

//Better Duplicate: compare only neighbors
public static boolean dup2(int[] A) {
  for (int i = 0; i < A.length - 1; i += 1) {
    if (A[i] == A[i + 1]) { 
      return true; 
    }
  }
  return false;
}
```

## Techniques for Measuring Computational Cost

**Technique 1**: Measure execution time in seconds using a client program \(i.e. actually seeing how quick our program runs in physical seconds\)

_Procedure_

* Use a physical stopwatch
* Or, Unix has a built in `time` command that measures execution time.
* Or, Princeton Standard library has a `stopwatch` class

_Observations_

* As our input size increases, we can see that `dup1` takes a longer time to complete, whereas `dup2` completes at relatively around the same rate.

_Pros vs. Cons_

* Pros: Very easy to measure \(just run a stopwatch\). Meaning is clear \(look at the actual length of time it takes to complete\).
* Cons: May take a lot of time to test. Results may also differ based on what kind of machine, compiler, input data, etc. you’re running your program with.

So how does this method match our goals? It's simple, so that's good, but not mathematically rigorous. Moreover, the differences based on machine, compiler, input, etc. mean that the results may not clearly demonstrate the relationship between dup1 and dup2.

How about another method?

### Technique 2

{% embed url="https://www.youtube.com/watch?v=fxDIy6w09fw&index=3&list=PL8FaHk7qbOD69aH2dhqcY64VMmX7frTUO" caption="" %}

**Technique 2A**: Count possible operations for an array of size N = 10,000.

```java
for (int i = 0; i < A.length; i += 1) {
  for (int j = i+1; j < A.length; j += 1) {
    if (A[i] == A[j]) {
       return true;
    }
  }
}
return false;
```

_Procedure_

* Look at your code and the various operations that it uses \(i.e. assignments, incrementations, etc.\)
* Count the number of times each operation is performed.

_Observations_

* Some counts get tricky to count.
* How did we get some of these numbers? It can be complicated and tedious.

_Pros vs. Cons_

* Pros: Machine independent \(for the most part\). Input dependence captured in model.
* Cons: Tedious to compute. Array size was arbitrary \(we counted for N = 10,000 — but what about for larger N? For a smaller N? How many counts for those?\). Number of operations doesn’t tell you the actual time it takes for a certain operation to execute \(some might be quicker to execute than others\).

So maybe this one has solved some of our cons from the timing simulation above, but it has problems of its own.

**Technique 2B**: Count possible operations in terms of input array size N \(symbolic counts\)

_Pros vs. Cons_

* Pros: Still machine independent \(just counting the number of operations still\). Input dependence still captured in model. But now, it tells us how our algorithm scales as a function of the size of our input.
* Cons: Even more tedious to compute. Still doesn’t tell us the actual time it takes!

**Checkpoint:** Applying techniques 2A and B to `dup2`

* Come up with counts for each operation, for the following code, with respect to N.
* Predict the _**rough**_ magnitudes of each one!

```java
for (int i = 0; i < A.length - 1; i += 1){
  if (A[i] == A[i + 1]) { 
    return true; 
  }
}
return false;
```

| **operation** | **symbolic count** | **count, N=10000** |
| :--- | :--- | :--- |
| **i = 0** | **1** | **1** |
| **less than \(&lt;\)** |  |  |
| **increment \(+=1\)** |  |  |
| **equals \(==\)** |  |  |
| **array accesses** |  |  |

**Answer**:

| **operation** | **symbolic count** | **count, N=10000** |
| :--- | :--- | :--- |
| **i = 0** | 1 | 1 |
| **less than \(&lt;\)** | 0 to N | 0 to 10000 |
| **increment \(+=1\)** | 0 to N - 1 | 0 to 9999 |
| **equals \(==\)** | 1 to N - 1 | 1 to 9999 |
| **array accesses** | 2 to 2N - 2 | 2 to 19998 |

Note: It's okay if you were slightly off — as mentioned earlier, you want _**rough**_ estimates.

## Checkpoint

{% embed url="https://youtu.be/IxQh6SXRBRw?list=PL8FaHk7qbOD69aH2dhqcY64VMmX7frTUO" caption="" %}

**Checkpoint**: Now, considering the following two filled out tables, which algorithm seems better to you and why?    
****`dup1`

| **operation** | **symbolic count** | **count, N=10000** |
| :--- | :--- | :--- |
| **i = 0** | 1 | 1 |
| **j = i + 1** | 1 to $$N$$ | 1 to 10000 |
| **less than \(&lt;\)** | 2 to \($$N^2+3N+2$$\)$$/2$$ | 2 to 50,015,001 |
| **increment \(+=1\)** | 0 to \($$N^2+N$$\)$$/2$$ | 0 to 50,005,000 |
| **equals \(==\)** | 1 to \($$N^2-N$$\)$$/2$$ | 1 to 49,995,000 |
| **array accesses** | 2 to $$N^2-N$$ | 2 to 99,990,000 |

`dup2`

| **operation** | **symbolic count** | **count, N=10000** |
| :--- | :--- | :--- |
| **i = 0** | 1 | 1 |
| **less than \(&lt;\)** | 0 to N | 0 to 10000 |
| **increment \(+=1\)** | 0 to N - 1 | 0 to 9999 |
| **equals \(==\)** | 1 to N - 1 | 1 to 9999 |
| **array accesses** | 2 to 2N - 2 | 2 to 19998 |

## Answer \(and Why Scaling Matters\)

{% embed url="https://youtu.be/jms0P6p6aQc?list=PL8FaHk7qbOD69aH2dhqcY64VMmX7frTUO" caption="" %}

`dup2` is better! But why?

* An answer: It takes fewer operations to accomplish the same goal.
* Better answer: Algorithm scales better in the worst case $$(N^2 + 3N + 2)/2$$ vs. $$N$$
* Even better answer: Parabolas $$N^2$$ grow faster than lines $$N$$
  * Note: This is the same idea as our “better” answer, but it provides a more general geometric intuition.

## Asymptotic Behavior

In most cases, we only care about what happens for very large N \(asymptotic behavior\). We want to consider what types of algorithms would best handle big amounts of data, such as in the examples listed below:

* Simulation of billions of interacting particles
* Social network with billions of users
* Encoding billions of bytes of video data

Algorithms that scale well \(i.e. look like lines\) have better asymptotic runtime behavior than algorithms that scale relatively poorly \(i.e. looks like parabolas\).

### Parabolas vs. Lines

{% embed url="https://www.youtube.com/watch?v=vxYADFsa3HU&list=PL8FaHk7qbOD69aH2dhqcY64VMmX7frTUO&index=6" caption="" %}

What about constants? If we had functions that took $$2N^2$$ operations vs. $$500N$$ operations, wouldn’t the one that only takes $$2N^2$$ operations be faster in certain cases, like if N = 4 \(32 vs. 20,000 operations\).

* Yes! For some small $$N$$, $$2N^2$$ may be smaller than $$500N$$. 
* However, as $$N$$ grows, the $$2N^2$$ will dominate. 
* i.e. N = 10,000 → 2\*100000000 vs. 5 \* 1000000

The important thing is the “shape” of our graph \(i.e. parabolic vs. linear\) Let us \(for now\) informally refer to the shape of our graph as the “orders of growth”.

## Returning to Duplicate Finding

Returning to our original goals of characterizing the runtimes of `dup1` vs. `dup2`

* They should be **simple** and **mathematically rigorous.**
* They should also clearly demonstrate the **superiority of dup2 over dup1.**

We’ve accomplished the second task! We were able to clearly see that `dup2` performed better than `dup1`. However, we didn’t do it in a very simple or mathematically rigorous way.

We did however talk about how `dup1` performed “like” a parabola, and `dup2` performed “like” a line. Now, we’ll be more formal about what we meant by those statements by applying the four simplifications.

### Intuitive Simplification 1: Consider only the Worst Case

When comparing algorithms, we often only care about the worst case \(though we'll see some exceptions later in this course\).

**Checkpoint**: Order of Growth Identification

Consider the counts for the algorithm below. What do you expect will be the order of growth of the runtime for the algorithm?

* $$N$$  \[linear\]
* $$N^2$$ \[quadratic\]
* $$N^3$$ \[cubic\]
* $$N^6$$ \[sextic\]

| **operation** | **count** |
| :--- | :--- |
| **less than \(&lt;\)** | $$100N^2+ 3N$$ |
| **greater than \(&gt;\)** | $$N^3+ 1$$ |
| **and \(&&\)** | $$5,000$$ |

**Answer**: It’s cubic \($$N^3$$\)!

* Why? Here’s an argument: 
* Suppose the $$<$$ operator takes $$\alpha$$ nanoseconds, the $$>$$ operator takes $$\beta$$ nanoseconds, and && takes $$\gamma$$ nanoseconds.
* Total time is $$\alpha(100N^2 + 3N) + \beta(2N^3 + 1) + 5000\gamma$$ nanoseconds.
* For very large N, the $$2 \beta N^3$$ term is much larger than the others.
  * You can think of it in terms of calculus if it helps.
  * What happens as N approaches infinity? When it becomes super large? Which term ends up dominating?
  * Very **important** point/observation to understand why this term is much larger!

### Intuitive Simplification 2: Restrict Attention to One Operation

Pick some representative operation to act as a proxy for overall runtime.

* Good choice: `increment`, or **less than** or **equals** or **array accesses**
* Bad choice: **assignment of** `j = i + 1`, or `i = 0`

The operation we choose can be called the “**cost model**.”

### Intuitive Simplification 3: Eliminate Low Order Terms

Ignore lower order terms!

**Sanity check**: Why does this make sense? \(Related to the checkpoint above!\)

### Intuitive Simplification 4: Eliminate Multiplicative Constants

Ignore multiplicative constants.

* Why? No real meaning!
* Remember that by choosing a single representative operation, we already “threw away” some information 
* Some operations had counts of $$3N^2$$, $$N^2/2$$, etc. In general, they are all in the family/shape of $$N^2$$!

This step is also related to the example earlier of $$500N$$ vs. $$2N^2$$.

## Simplification Summary

* Only consider the worst  case.
* Pick a representative operation \(aka: cost model\)
* Ignore lower order terms
* Ignore multiplicative constants.

**Checkpoint**: Apply these four steps to `dup2`, given the following tables.

| **operation** | **count** |
| :--- | :--- |
| **i = 0** | 1 |
| **less than \(&lt;\)** | 0 to N |
| **increment \(+=1\)** | 0 to N - 1 |
| **equals \(==\)** | 1 to N - 1 |
| **array accesses** | 2 to 2N - 2 |

| **operation** | **worst case orders of growth** |
| :--- | :--- |
|  |  |

**Sample Answer:** `Array accesses` \| `N`, or `less than/increment/equals` \|`N`

### **Summary of our \(Painful\) Analysis Process**

* Construct a table of exact counts of all possible operations \(takes lots of effort!\)
* Convert table into worst case order of growth using 4 simplifications.

But, what if we just avoided building the table from the get-go, by using our simplifications from the very start?

## Simplified Analysis Process

{% embed url="https://www.youtube.com/watch?v=lJ1A8Jyeba0&list=PL8FaHk7qbOD69aH2dhqcY64VMmX7frTUO&index=7" caption="" %}

Rather than building the entire table, we can instead:

* Choose our cost model \(representative operation we want to count\).
* Figure out the order of growth for the count of our representative operation by either:
  * Making an exact count, and discarding unnecessary pieces
  * Or, using intuition/inspection to determine orders of growth \(comes with practice!\)

We’ll now re-analyze `dup1` using this process.

### Analysis of Nested For Loops: Exact Count

Find order of growth of worst case runtime of `dup1`.

```java
int N = A.length;
for (int i = 0; i < N; i += 1)
   for (int j = i + 1; j < N; j += 1)
      if (A[i] == A[j])
         return true;
return false;
```

**Cost model**: number of == operations

Given the following chart, how can we determine how many == occurs? The y axis represents each increment of i, and the x access represents each increment of j. ![](../.gitbook/assets/8.1-==-chart.png)

* Worst case number of == operations: 
  * Cost = 1 + 2 + 3 + … + \(N-2\) + \(N-1\)
* How do we sum up this cost? 
* Well, we know that it can also be written as:
  * Cost = \(N-1\) + \(N-2\) + … + 3 + 2 + 1
* Let’s sum up these two Cost equations:
  * 2\*Cost = N + N + N + … N
* How many N terms are there? 
  * N-1! \(the pairs that summed up to N, through adding the two Cost equations together\)
* Therefore: 2\*Cost = N\(N-1\)
* Therefore: Cost = N\(N-1\)/2
* If we do our simplification \(throwing away lower order terms, getting rid of multiplicative constants\), we get worst case orders of growth = $$N^2$$ 

### Analysis of Nested For Loops: Geometric Argument

* We can see that the number of equals can be given by the area of a right triangle, which has a side length of N - 1
* Therefore, the order of growth of area is $$N^2$$ 
* Takes time and practice to be able to do this!

[https://www.youtube.com/watch?v=sMlmXdKb9fA&list=PL8FaHk7qbOD69aH2dhqcY64VMmX7frTUO&index=8](https://www.youtube.com/watch?v=sMlmXdKb9fA&list=PL8FaHk7qbOD69aH2dhqcY64VMmX7frTUO&index=8)

## Formalizing Order of Growth

Given some function, Q\(N\), we can apply our last two simplifications to get the order of growth of Q\(N\).

* Reminder: last two simplifications are dropping lower order terms and multiplicative constants.
* Example: $$Q(N) = 3N^3 + N2$$
* After applying the simplifications for order of growth, we get: $$N^3$$

Now, we’ll use the formal notation of “Big-Theta" to represent how we’ve been analyzing our code.

**Checkpoint**: What’s the shape/orders of growth for the following 5 functions?

| **function** | **order of growth** |
| :--- | :--- |
| $$N^3 + 3N^4$$ |  |
| $$1/N + N^3$$ |  |
| $$1/N + 5$$ |  |
| $$Ne^N+ N$$ |  |
| $$40 sin(N) + 4N^2$$ |  |

**Answer:**

| **order of growth** |
| :--- |
| $$N^4$$ |
| $$N^3$$ |
| $$1$$ |
| $$Ne^N$$ |
| $$N^2$$ |

## Big-Theta

{% embed url="https://youtu.be/CGdubALgQw4" caption="" %}

Suppose we have a function R\(N\) with order of growth f\(N\). In “Big-Theta” notation we write this as R\(N\) \in \Theta\(f\(N\)\). This notation is the formal way of representing the "families" we've been finding above.

Examples \(from the checkpoint above\):

* $$N^3 + 3N^4$$ $$\in \Theta(N^4)$$
* $$1/N + N^3 \in \Theta(N^3)$$
* $$1/N + 5 \in \Theta(1)$$
* $$Ne^N + N \in \Theta(Ne^N)$$
* $$40 sin(N) + 4N^2 \in \Theta(N^2)$$

#### Formal Definition

$$R(N) \in \Theta(f(N))$$ means that there exists positive constants $$k_1, k_2$$ such that:  
$$k_1 \cdot f(N) \leq R(N) \leq k_2 \cdot f(N)$$, for all values of N greater than some $$N_0$$ \(a very large N\).

#### Big-Theta and Runtime Analysis

Using this notation doesn’t change anything about how we analyze runtime \(no need to find the constants $$k_1, k_2$$\)!

The only difference is that we use the $$\Theta$$ symbol in the place of “order of growth” \(i.e. worst case runtime: $$\Theta(N^2)$$\)

## Big O

{% embed url="https://youtu.be/yG5mYNR3aIU" caption="" %}

Earlier, we used Big Theta to describe the order of growth of functions as well as code pieces. Recall that if $$R(N) \in \Theta(f(N))$$, then $$R(N)$$ is both upper and lower bounded by $$\Theta(f(N))$$. Describing runtime with both an upper and lower bound can informally be thought of as runtime "equality".

Some examples:

| function $$R(N)$$ | Order of growth |
| :--- | :--- |
| $$N^3 + 3N^4$$ | $$\Theta(N^4)$$ |
| $$N^3 + 1/N$$ | $$\Theta(N^3)$$ |
| $$5 + 1/N$$ | $$\Theta(1)$$ |
| $$Ne^N + N$$ | $$\Theta(Ne^N)$$ |
| $$40 \sin(N) + 4N^2$$ | $$\Theta(N^2)$$ |

For example, $$N^3 + 3N^4 \in \Theta(N^4)$$. It is both upper and lower bounded by $$N^4$$.

On the other hand, Big O can be though of as a runtime inequality, namely, as "less than or equal". For example, all of the following are true: $$N^3 + 3N^4 \in O(N^4)$$ $$N^3 + 3N^4 \in O(N^6)$$ $$N^3 + 3N^4 \in O(N!)$$ $$N^3 + 3N^4 \in O(N^{N!})$$

In other words, if a function, like the one above, is upper bounded by $$N^4$$, then it is also upper bounded by functions that themselves upper bound $$N^4$$. $$N^3 + 3N^4$$ is "less than or equal to" all of these functions in the asymptotic sense.

Recall the formal definition of Big Theta: $$R(N) \in \Theta(f(N))$$ means that there exists positive constants $$k_1, k_2$$ such that:  
$$k_1 \cdot f(N) \leq R(N) \leq k_2 \cdot f(N)$$ for all values of N greater than some $$N_0$$ \(a very large N\).

#### Formal Definition

Similarly, here's the formal definition of Big O: $$R(N) \in O(f(N))$$ means that there exists positive constants $$k_2$$ such that:  
$$R(N) \leq k_2 \cdot f(N)$$ for all values of N greater than some $$N_0$$ \(a very large N\).

Observe that this is a looser condition than Big Theta since Big O does not care about the lower bound.

## Summary

* Given a piece of code, we can express its runtime as a function R\(N\)
  * N is some **property** of the input of the function 
  * i.e. oftentimes, N represents the **size** of the input
* Rather than finding R\(N\) exactly, we instead usually only care about the **order of growth** of R\(N\).
* One approach \(not universal\):
  * Choose a representative operation
  * Let C\(N\) = count of how many times that operation occurs, as a function of N.
  * Determine order of growth $$f(N)$$ for $$C(N)$$, i.e. $$C(N) \in \Theta(f(N))$$ 
  * Often \(but not always\) we consider the worst case count.
  * If operation takes constant time, then $$R(N) \in \Theta(f(N))$$

