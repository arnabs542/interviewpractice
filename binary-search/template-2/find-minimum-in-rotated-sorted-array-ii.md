# Find Minimum In Rotated Sorted Array II

```py
def findMin(self, nums):

    lo, hi = 0, len(nums) - 1
    while lo < hi:

        mid = (lo + hi) // 2
        if nums[mid] > nums[hi]:
            lo = mid + 1
        elif nums[mid] < nums[hi]:
            hi = mid
        else:
            if nums[hi - 1] > nums[hi]:
                return nums[hi]
            hi -= 1
    return nums[lo]
```

Same idea as original version. However, with duplicates, we can't make an immediate judgement regarding where the inversion point lies relative to `mid`. In the event that `nums[mid] == nums[hi]`, we must decrease `hi` until we gain enough information to make a judgement.

One thing to note is that without the `if` statement, we could return the right value, but not necessarily the right index.

For example, take the array \[1, 1, 1, 1, 1, 1, 1, 1, 2, 1, 1\]. We will end up returning 0, even though the real "minimum value index" should be 9, since it's the first appearance of the minimum value.

Therefore, to ensure absolute correctness, we need to check if `nums[hi] < nums[hi - 1]`, and return immediately if true.

Note that we can't really use `nums[lo]` to make any decisions here. If `nums[lo] > nums[mid]`, then we know that the inversion points lies to the left of or at `mid`. However, when `nums[lo] < nums[mid]`, we can't say exactly where the inversion point is. On the other hand, if `nums[hi] > nums[mid]`, we know that the inversion point lies to the left of or at `mid`, and if `nums[hi] < nums[mid]`, we know that the inversion points lies to the right of `mid`.

