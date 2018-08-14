# Search in Rotated Sorted Array

```py
def search(self, nums, target):

    lo, hi = 0, len(nums) - 1
    while lo <= hi:
        mid = (lo + hi) // 2
        if nums[mid] == target:
            return mid
        elif nums[mid] < target:
            if target <= nums[hi] or nums[mid] > nums[hi]:
                lo = mid + 1
            else:
                hi = mid - 1
        else:   # nums[mid] > target
            if target >= nums[lo] or nums[lo] > nums[mid]:
                hi = mid - 1
            else:
                lo = mid + 1
    return -1
```

Again, notice the key attributes of the solution compared to those of template 1:

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

We start with a search space of the nums array. Since every single number in that array is possibly a candidate, we need to make our bounds inclusive. Furthermore, we should test every single element, which means our terminating condition will be set to `left > right`.

The boundary updates requires a bit of thought. In a normal binary search, we know that each section on either side of mid is sorted. However, in this array, there could be at most one "inversion point" i, for which the condition nums\[i\] &lt; nums\[i - 1\] holds true. Because of this, we need to do a bit more work to figure out how to accurately update our bounds.

Suppose that nums\[mid\] &lt; target. In a normal sorted array, we would shift lo to mid + 1. However, the increasing portion could wrap all the way up to the front, and if target is large enough, we may actually need to search in the first half of the array. Therefore, we consider all possible cases in which we would want to search in the second half of the search space:

1. `target <= nums[hi]`      Since the array is sorted, then we know that `nums[mid : hi]` is increasing order, and we need to move `lo` up. Notice that we must compare target to `nums[hi]` with`<=`, because if `nums[hi] == target`, we would end up searching in the wrong space without the correct comparison. 
2. `nums[mid] > nums[hi]`    Since all values are unique, this means that the inversion point happens in the second half the array, which in means that the interval `nums[lo : mid]` is sorted and the largest elements are in the second half of the array. 

Otherwise, the answer must be in the first half of array.

We apply the same sort of case analysis for when we encounter situations when `nums[mid] > target`.

