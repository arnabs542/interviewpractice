# Find Minimum in Rotated Sorted Array

```py
def findMin(self, nums):

    lo, hi = 0, len(nums) - 1
    while lo < hi:
        mid = (lo + hi) // 2
        if nums[mid] < nums[hi]:
            hi = mid
        else:
            lo = mid + 1
    return nums[lo]
```

This problem is slightly related to the search in rotated sorted array - the difference here is that we're not searching for a specific value, but the smallest value.

The above solution takes elements from both template 1 and 2. We still use an inclusive search space, since all the elements in the array could be the answer. However, we terminate when we still have 1 element left in the search space.

We move `hi = mid` instead of `mid - 1` because `mid` itself could be the answer. We don't worry about infinite loops because `mid` is always rounded down, so it will never equal `hi`. When we reach `lo == hi`, we terminate.

To summarize some key points:

1. Loop termination condition is`left < right`, which means inside the loop, `left` always &lt;`right`
2. Since we round down for `mid`, and `left < right` from \(1\), `right`** would never be the same as **`mid`
3. Therefore, we compare `mid` with `right`, since they will never be the same from \(2\)

4. If `nums[mid] < nums[right]`, we will know the minimum should be in the left part, so we are moving `right`

   * We can always make `right = mid` without having to worry about infinite loops. Since from \(2\), we know `right` would never be the same as `mid`, making `right = mid` will assure the interval is shrinking.

5. If `nums[mid] > nums[right]`, minimum should be in the right part, so we are moving `left`. Since `nums[mid] > nums[right]`,  
   `mid` can't be the minimum, we can safely move `left` to `mid + 1`, which also assure the interval is shrinking

In all binary search problems, we need to consider:

* Initial bounds. Inclusive or exclusive?
* Termination condition. How many elements left before we exit the loop?
* Avoid infinite loops. How do we adjust the boundaries so each iteration there is new information for us to process?



