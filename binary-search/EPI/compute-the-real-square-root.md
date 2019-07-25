# Compute the Real Square Root

First the code:

```py
def square_root(x):
    left, right = (x, 1.0) if x < 1.0 else (1.0, x)
    while not math.isclose(left, right):
        mid = 0.5 * (left + right)
        mid_squared = mid * mid
        if mid_squared > x:
            right = mid
        else:
            left = mid
    return left
```

The first thing to realize was the when $\small x < 1.0$, the square root of $\small x$ would actually be larger than $\small x$, e.g. $\small \sqrt{0.25} = 0.5$. Therefore, depending on the value of $\small x$, we need to establish different solution intervals.

The next part was the usage of `math.isclose`, which determines how close two values are. Per the official Python doc:

> Return `True` if the values _a_ and _b_ are close to each other and `False` otherwise.
>
> Whether or not two values are considered close is determined according to given absolute and relative tolerances.
>
> _rel\_tol_ is the relative tolerance – it is the maximum allowed difference between _a_ and _b_, relative to the larger absolute value of _a_ or _b_. For example, to set a tolerance of 5%, pass `rel_tol=0.05`. The default tolerance is `1e-09`, which assures that the two values are the same within about 9 decimal digits. _rel\_tol_ must be greater than zero.

In this situation, we're actually able to update `left` and `right` directly to `mid` without worry of infinite loops. This is because before, when we used `mid = (left + right) // 2`, `mid` was always truncated to the closest smallest integer value. This meant that `mid` could equal `left` or `right`. However, in a floating evaluation, `mid` will never equal `left` or `right`, and this we can set the boundaries directly to `mid` and still be assured that we are constantly shrinking the interval. Time complexity is $\small \mathcal O(\log(\frac{x}{s}))$, where $\small s$ is the tolerance. 

