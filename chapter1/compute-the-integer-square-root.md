# Compute the Integer Square Root

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



