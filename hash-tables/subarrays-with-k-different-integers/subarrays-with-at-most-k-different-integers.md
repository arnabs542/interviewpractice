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



