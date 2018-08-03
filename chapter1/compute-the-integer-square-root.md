# Compute the Integer Square Root

The brute force solution is to iterate from 1 to $$\lceil(\frac{k}{2})\rceil$$ and return the preceding integer of the first number whose square goes over k. Note the because `k // 2`is the floor function, we need to `k//2 + 2` as our upper bound. This takes $$\mathcal{O}(n)$$ time.

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

We iniitialize the interval to $$[0, k]$$. We compare the square of $$m = \lfloor(l+2)/2)\rfloor$$ with $$k$$, and use the elimination rule to update the interval. If $$m^{2} \leq k$$, we know that all integers less than or equal to $$m$$ have an square less than or equal to $$k$$. Therefore, we update the interval to $$[m + 1, r]$$. If $$m^{2} > k$$, we know all numbers greater than or equal to $$m$$ have a square greater than $$k$$, so we update the candidate interval to $$[l, m - 1]$$. The algorithm terminates when the interval is empty, in which case every number less than $$l$$ has a square less than or equal to $$k$$ and $$l's$$ square is greater than $$k$$, so the result is $$l - 1$$. 

For example, if $$k = 21$$, we initialize the interval to $$[0,21]$$. The midpoint $$m = \lfloor(0 + 21)/2\rfloor = 10$$; since $$10^{2} > 21$$, we update the interval to $$[0, 9]$$. Now $$m = \lfloor\(0 + 9\)/2\rfloor = 4$$

$$\ceil{1 over 2}$$

```
Here is some inline math: $$a \ne 0$$
```

$$a \ne 0$$

$$$$

