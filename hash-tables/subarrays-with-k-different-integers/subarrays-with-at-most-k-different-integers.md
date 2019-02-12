#### Subarrays with At Most K Different Integers

> Given an array `A` of positive integers, call a \(contiguous, not necessarily distinct\) subarray of `A`_good_ if the number of different integers in that subarray is at most `K`.
>
> \(For example, `[1,2,3,1,2]` has `3` different integers: `1`, `2`, and `3`.\)
>
> Return the number of good subarrays of `A`.

##### Sliding Window:

```py
def subarraysWithAtMostK(A: 'List[int]', K: 'int') -> 'int':
    window = collections.defaultdict(int)
    res = i = j = 0
    while j < len(A):
        if window[A[j]] == 0:
            K -= 1
        window[A[j]] += 1
        while K < 0:
            window[A[i]] -= 1
            if window[A[i]] == 0:
                K += 1
            i += 1
        res += j - i + 1
        j += 1
    return res
```

This problem is really the key to solving the above problem.

We use a sliding window to keep track of the number of distinct elements in the current window. If less than or equal to $$\small K$$, we do `res += j - i + 1` to add the number number of valid subarrays.

Suppose we have an array of $$\small [1,2,3]$$. This array has 6 subarrays: $$\small [1], [2], [3], [1,2], [2,3], [1,2,3]$$. If we append an additional elements, $$\small 4$$ to the array, 4 additional subarrays are introduced: $$\small [4], [3,4], [2,3,4], [1,2,3,4]$$. In other words, suppose a window of $$\small n$$ elements contains less than $$\small K$$ distinct elements. Suppose also that the $$\small n+1$$ window made by introducing a new element has less than or equal to $$\small K$$ distinct elements. In this case, $$\small n+1$$ additional valid subarrays are added.

Another way to look at this is by combinations. For a valid window of size $$\small n$$, there are $$\small {{n+1}\choose{2}} = \frac{n(n+1)}{2}$$. 

