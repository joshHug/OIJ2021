# 9.5 WQU with Path Compression

Weighted Quick Union is pretty good, but we can do even better!

{% embed url="https://youtu.be/DZKzDebT4gU" caption="" %}

The clever insight is realizing that whenever we call `find(x)` we have to traverse the path from `x` to root. So, along the way we can connect all the items we visit to their root at no extra asymptotic cost.

Connecting all the items along the way to the root will help make our tree shorter with each call to `find`.

Recall that **both `connect(x, y)` and `isConnected(x, y)` always call `find(x)` and `find(y)`.** Thus, after calling `connect` or `isConnected` enough, essentially all elements will point directly to their root.

By extension, the average runtime of `connect` and `isConnected` becomes **almost constant** in the long term! This is called the _amortized runtime_ \(from [amortized analysis, ch. 8.4](https://joshhug.gitbooks.io/hug61b/content/chap8/chap84.html#amortized)\).

More specifically, for M operations on N elements, WQU with Path Compression is in O\(N + M \(lg\* N\)\). lg\* is the [iterated logarithm](https://en.wikipedia.org/wiki/Iterated_logarithm) which is less than 5 for any real-world input. 

## Summary

N: number of elements in Disjoint Set

| Implementation | `isConnected` | `connect` |
| :--- | :--- | :--- |
| Quick Find | Θ\(N\) | Θ\(1\) |
| Quick Union | O\(N\) | O\(N\) |
| Weighted Quick Union \(WQU\) | O\(log N\) | O\(log N\) |
| WQU with Path Compression | O\(α\(N\)\)\* | O\(α\(N\)\)\* |

\*behaves as constant in long term.

Code? This is your [lab 6](https://sp19.datastructur.es/materials/lab/lab6/lab6)!

