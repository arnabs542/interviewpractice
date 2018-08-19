# Guess Number Higher or Lower

A very standard application of the template:

```py
def guessNumber(self, n):

    lo, hi = 1, n
    while lo <= hi:
        mid = (lo + hi) // 2
        if guess(mid) == 0:
            return mid
        elif guess(mid) == 1:
            lo = mid + 1
        else:
            hi = mid - 1
```

> **Key Attributes:**
>
> * Initial bounds are inclusive
> * Search Condition can be determined without comparing to the element's neighbors \(or use specific elements around it\)
> * No post-processing required because the termination condition requires `left > right`, which means there are no candidates left in the search space when the loop terminates. 
>
> **Distinguishing Syntax:**
>
> * Initial Condition: `left = 0, right = length-1`
> * Termination: `left > right`
> * Searching Left: `right = mid-1`
> * Searching Right:`left = mid+1`
>
> We use an inclusive bound \[1, n\], then we exhaust the entire search space until left &gt; right.



