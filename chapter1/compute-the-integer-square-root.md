# Compute the Integer Square Root

The brute force solution is to iterate from 1 to $$\lceil(\frac{k}{2}\rceil)$$ and return the preceding integer of the first number whose square goes over k. Note the because `k // 2`is the floor function, we need to `k//2 + 2` as our upper bound. This takes $$\mathcal{O}(n)$$ time. 

```py
def square_root(k):
    if k < 2:
        return k
    for i in range(1, k//2 + 2):
        if i * i > k:
            return i - 1
```

Below is the binary search solution. 

```py
def square_root(k):
    if k < 2:
        return k
    lo, hi = 1, k // 2
    while lo < hi:
        mid = (lo + hi) // 2
        if mid > (k // mid):
            hi = mid
        elif mid + 1 > (k // (mid + 1)):
            return mid
        else:
            lo = mid + 1
    return lo
```

The intuition comes from the realization that if $$x^{2} < k$$, there is no point in checking any number smaller than $$x$$. Similarly, if $$x^{2} > k$$, then there is no point in checking any number larger than $$x$$. **The ability to discard large chunks of the search space is usually and indicator that this could be a binary search problem.** Specifically, we want to maintain a search space that consist of values whose squares are unclassified with respect to $$k$$, i.e. might be less than or greater than $$k$$.

We iniitialize the interval to `[0, k]`. We compare the square 



$$\ceil{1 over 2}$$

```
Here is some inline math: $$a \ne 0$$
```

$$a \ne 0$$

$$$$

