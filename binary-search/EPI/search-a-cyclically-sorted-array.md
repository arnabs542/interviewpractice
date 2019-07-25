# Search a Cyclically Sorted Array

My first attempted was a modified version of Template 1 \(refer to Leetcode section on binary search\), which is the inclusive upper bound. We proceed by looking for where the inversion point lies.

```py
def search_smallest(A):
    lo, hi = 0, len(A) - 1
    while lo < hi:
        if A[lo] < A[hi]:
            return lo
        mid = (lo + hi) // 2
        if A[lo] > A[mid]:
            hi = mid - 1
        else:
            lo = mid 
    return lo
```

First we check if `A[lo] < A[hi]`. If this is true, then that means there's no inversion in this array, and we can return lo.

Next we find mid. My thinking was that in the case where `A[lo] > A[mid]`is True, there must be an inversion point before or at the midpoint. Otherwise, we move lo up to mid, since mid could be the inversion point.

Take for example the array:

`[3,1,2]`.

Our iterations will proceed as follows:
>```
> lo    hi    mid
>
> 0    2    1
>
> 0    0
>```

In this case, when the inversion point occurs on mid, by setting `hi = mid - 1` we actually end up skipping over the answer. However, if we leave `hi = mid`, our iterations will proceed as follows:
>```
> lo    hi    mid
>
> 0    2    1
>
> 0    1    0
>
> 0    1    0
> ```
We end up going in an infinite loop.

I then tried using Template 1 even more literally:

```py
def search_smallest(A):
    lo, hi = 0, len(A) - 1
    while lo <= hi:
        if A[lo] < A[hi]:
            return lo
        mid = (lo + hi) // 2
        if A[lo] > A[mid]:
            hi = mid - 1
        else:
            lo = mid + 1
    return lo
```

This passed more tests, but failed on the following input: `[10, 20, 30, 40, 5]`
>```
> lo    hi    mid
>
> 0    4    2
>
> 3    4    3
>
> 4    4    4
>```
This fails on the initial termination condition, since `lo <= hi` forces one more check, which ends up bringing `lo` to 5, which is out of bounds. However, by replacing &lt;= with &lt;, we fail on the first input \[3,1,2\] again, since by setting `hi = mid - 1` we end up skipping over the inversion point when it lies directly on mid.

In the end, I stumbled onto the solution by accident. We can set `hi = mid` to avoid skipping over the inversion point, but to prevent us from being locked in a loop we set `lo = mid + 1` regardless. This makes sense because if `A[lo] > A[mid]` is false, that means `A[mid] >= A[lo]`, which also means that there is no possible situation in which `A[mid]` could be the smallest element in the array. Therefore, we can discard it in our bounds.

