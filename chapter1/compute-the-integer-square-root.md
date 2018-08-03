# Compute the Integer Square Root

The brute force solution is to iterate from 1 to $$\ceil[k \over 2]$$ and return

```py
def square_root(k):
    if k < 2:
        return k
    for i in range(1, k//2 + 2):
        if i * i > k:
            return i - 1
```

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

The quadratic formula is $$-b \pm \sqrt{b^2 - 4ac} \over 2a$$

dsfsdf

$$\ceil[\big]{\frac{1}{2}}\]$$

