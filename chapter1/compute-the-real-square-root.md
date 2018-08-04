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

The first thing to realize was the when $$\small x < 1.0$$, the square root of $$\small x $$ would actually be larger than $$\small x$$, e.g. $$\small \sqrt{0.25} = 0.5$$. Therefore, depending on the value of $$\small x$$, we need to establish different solution intervals.

The next part was the 

