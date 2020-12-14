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

The intuition for time optimization comes from the realization that if $$\small x^{2} < k$$, there is no point in checking any number smaller than $$x$$. Similarly, if $$\small x^{2} > k$$, then there is no point in checking any number larger than $$x$$. **The ability to discard large chunks of the search space is usually and indicator that this could be a binary search problem.** Specifically, we want to maintain a search space that consist of values whose squares are unclassified with respect to $$k$$, i.e. might be less than or greater than $$k$$.

Below is one implementation of the binary search solution.

```py
def square_root(k):
    if k < 2:
        return k
    lo, hi = 1, k // 2 + 1
    while lo <= hi:
        mid = (lo + hi) // 2
        if mid > (k // mid):
            hi = mid - 1
        else:
            lo = mid + 1
    return hi
```

Again, the general intuition here is that we want to find the largest number$$\small m$$ for which $$\small m * m < k$$ holds true. If $$\small m > \lfloor \frac{k}{m} \rfloor$$, then $$\small m $$ is strictly too big, and we can move `hi` to $$\small m - 1$$. Otherwise, we move `lo` up. By the time we terminate, `hi` should be smaller of the two numbers, and should hold the answer.

We want to set our terminating condition to `lo <= hi` because we need to check the final number when both of these converge to a single number, since that could be answer. If we set the terminating condition strictly to `lo < hi`, when `lo == hi` we will end up not checking that number.

Below is the book solution:

```py
def square_root(k):
    left, right = 0, k
    # Candidate interval [left, right] where everything before left has square
    # <= k, everything after right has square > k.
    while left <= right:
        mid = (left + right) // 2
        mid_squared = mid * mid
        if mid_squared <= k:
            left = mid + 1
        else:
            right = mid - 1
    return left - 1
```

We iniitialize the interval to $$\small [0, k]$$. We compare the square of $$\small m = \lfloor(l+r)/2)\rfloor$$ with $$k$$, and use the elimination rule to update the interval. If $$\small m^{2} \leq k$$, we know that all integers less than or equal to $$m$$ have an square less than or equal to $$k$$. Therefore, we update the interval to $$\small [m + 1, r]$$. If $$\small m^{2} > k$$, we know all numbers greater than or equal to $$m$$ have a square greater than $$k$$, so we update the candidate interval to $$\small [l, m - 1]$$. The algorithm terminates when the interval is empty, in which case every number less than $$l$$ has a square less than or equal to $$k$$ and $$l's$$ square is greater than $$k$$, so the result is $$l - 1$$.

For example, if $$\small k = 21$$, we initialize the interval to $$\small [0,21]$$. The midpoint $$\small m = \lfloor(0 + 21)/2\rfloor = 10$$; since $$\small 10^{2} > 21$$, we update the interval to $$\small [0, 9]$$. Now $$\small m = \lfloor(0 + 9\rfloor = 4$$; since $$\small 4^{2} < 21$$, we update the interval to $$\small [5,9]$$. Now $$\small m = \lfloor(5 + 9\rfloor = 7$$; since $$\small 7^{2} < 21$$, we update the interval to $$\small [5,6]$$. Now $$\small m = \lfloor(5 + 6\rfloor = 5$$; since $$\small 5^{2} < 21$$, we update the interval to $$\small [5,4]$$. Now the right endpoint is than the left endpoint, i.e., the interval is empty, so the result is $$\small 5 -1 = 4 $$, which is the value returned.

For $$\small k = 25$$, the sequence of intervals is $$\small [0, 25], [0, 11], [6, 11], [6, 7], [6,5]$$. The returned value is $$\small 6 -1 = 5$$.

Running time is $$\mathcal{O}(\log{n})$$.

