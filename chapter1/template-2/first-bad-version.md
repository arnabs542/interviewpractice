# First Bad Version

```py
def firstBadVersion(n):

    lo, hi = 1, n
    ans = -1
    while lo <= hi:
        mid = (lo + hi) // 2
        if isBadVersion(mid):
            ans = mid
            hi = mid - 1
        else:
            lo = mid + 1
    return ans
```

The above solution actually follows template 1. We start with inclusive bounds \[1, n\]. We terminate when lo &gt; hi, meaning we exhaust all candidates in our search space. This is similar to the question _Find First Occurrence of k in a Sorted Array_. We see if `mid` is a bad version. If so, we remember it, and we continue searching in the first half. Else, we search in the second half.

```py
def firstBadVersion(self, n):

    lo, hi = 1, n
    while lo <= hi:
        if isBadVersion(lo):
            return lo
        mid = (lo + hi) // 2
        if isBadVersion(mid):
            hi = mid
        else:
            lo = mid + 1
```

While this is also closer to template 1, there's a few important optimizations made here.

The main idea is the same: inclusive search space of `[1, n] => [lo, hi]`. The maker lo will contain the first possible version that could be bad, and we run a check on that first. If lo is bad, then we return, since `lo` is the earliest possible number. On the other hand, `hi` is the last version which could be bad. Therefore, when we find a `mid`that is a bad version, we set `hi = mid`. Because `mid`is always rounded down, `mid` will almost never equal `hi`. Therefore, by setting hi = mid, we're still at least shrinking the search space by 1. In the event that `lo == mid == hi`, we will end up catching the infinite loop with the `isBadVersion(lo)` check.

