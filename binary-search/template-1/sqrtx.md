# Sqrt\(x\)

This solution follows the template very strictly:

```py
def mySqrt(self, x):

    lo, hi = 0, x
    while lo <= hi:
        mid = (lo + hi) // 2
        if mid * mid > x:
            hi = mid - 1
        elif mid * mid == x:
            return mid
        else:
            lo = mid + 1
    return lo - 1
```

In fact, we don't even really need the `elif` statement:

```py
def mySqrt(self, x):

    lo, hi = 0, x
    while lo <= hi:
        mid = (lo + hi) // 2
        if mid * mid > x:
            hi = mid - 1
        else:
            lo = mid + 1
    return lo - 1
```

The idea here is that the square root of x will be guaranteed to lie in the range of \[0, x\], if x &gt;= 0. Therefore, that will be our initial search space.

Given a number mid, suppose that $\small mid * mid > x$. We then know that `[mid : hi]` will not contain the answer. In the event that $\small mid * mid < x$, we know that the answer will lie in `[mid + 1 : hi]`. The goal here is to classify all numbers in the search space by shifting our bounds hi and lo. Our termination condition will be when `lo > hi`, and since we move lo upwards whenever our current mid isn't enough, we know that `lo - 1` will be the answer.  We can also return `hi`, since that will be 1 smaller than `lo`.

We will always end up converging onto a search space of 1 at the end, when `lo = mid = hi`. Since `mid` is always truncated to the floor, it will end up equaling `lo`. Therefore, `lo` will always end at `hi + 1`.

