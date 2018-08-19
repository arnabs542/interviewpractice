# Search in Rotated Sorted Array II

```py
def search(self, nums, target):

    lo, hi = 0, len(nums) - 1
    while lo <= hi:
        mid = (lo + hi) // 2
        if nums[mid] == target:
            return True
        if nums[mid] == nums[hi]:
            hi -= 1
            continue
        if nums[mid] < target:
            if target <= nums[hi] or nums[mid] > nums[hi]:
                lo = mid + 1
            else:
                hi = mid - 1
        else:
            if nums[lo] <= target or nums[lo] > nums[mid]:
                hi = mid - 1
            else:
                lo = mid + 1
    return False
```

This problem differs from the original one in that there can be duplicates. For example, the nums array can now be `[2,2,1,2,2]`.

Our solution for the no duplicate version fails here because there is no way to tell where the inversion point. For example, given the above array, our first iteration will only reveal this array to be like so: `[2, x, 1, x, x, 2]`. The inversion point can either be in the left or the right, e.g. the array may be like `[2, 3, 1, 2, 2, 2]` or `[2, 0, 1, 2, 2, 2]`. Therefore, worst case time complexity is $$\small \mathcal{O}(n)$$ for this problem.

In other words, the introduction of duplicates means that simple comparisons between `nums[mid]` and `nums[lo]`, `nums[hi]` no longer tells us enough regarding the sortedness of the two subarrays. We will need to make incremental changes until we can discard large sections of the search space.

We can try to mitigate by applying binary search whenever we're able to gather enough information regarding the array. We add an extra check condition in the even that `nums[mid] == nums[hi]`, we just decrease `hi` until the elements are no longer the same and we can make a judgment on what half of the search space to throw away.

We can also increase `lo` until `nums[lo] != nums[mid]`, or increase `lo` and decrease `hi`simultaneously until we have enough information to continue.

