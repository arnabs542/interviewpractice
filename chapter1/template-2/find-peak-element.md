# Find Peak Element

```py
def findPeakElement(self, nums):
    lo, hi = 0, len(nums) - 1
    while lo < hi:
        mid = (lo + hi) // 2
        if nums[mid] > nums[mid + 1]:
            hi = mid
        else:
            lo = mid + 1
    return lo
```

In this problem, we are told that `nums[-1] = nums[n] = -∞`. Given these two conditions, we can be assured that there must be a peak element.

We initialize `left = 0`, `right = len(nums) - 1`. The invariant is: **nums\[left - 1\] &lt; nums\[left\] && nums\[right\] &gt; nums\[right + 1\]**

What this means is that in the current interval we're looking, \[left, right\], the function starts increasing to the left and will eventually decrease at right. The behavior between \[left, right\] falls into the following 3 categories:

1. `nums[left] > nums[left + 1]`. From the invariant, `nums[left - 1] < nums[left]` =&gt; left is a peak

2. The function is increasing from left to right i.e. `nums[left] < nums[left + 1] < .. < nums[right - 1] < nums[right]`. From the invariant, `nums[right] > nums[right + 1]` =&gt; `right` is a peak

3. the function increases for a while and then decreases \(in which case the point just before it starts decreasing is a peak\) e.g. `[2 5 6 3]` \(6 is the point in question\)

As shown, if the invariant above holds, there is at least a peak between \[left, right\]. Now we need to show 2 things:

1. the invariant is initially true. Since `left = 0` and `right = len(nums) - 1` initially and we know that `nums[-1] = nums[n] = -oo`, this is obviously true
2. At every step of the loop the invariant gets reestablished. If we consider the code in the loop, we have `mid = (left + right) // 2` and the following 2 cases:  
   1. `nums[mid] < nums[mid + 1]`. It turns out that the interval `[mid + 1, right]` respects the invariant \(`nums[mid] < nums[mid + 1]` -&gt; part of the cond + `nums[right] > nums[right + 1]` -&gt; part of the invariant in the previous loop iteration\)

   1. `nums[mid] > nums[mid + 1]`. Similarly, `[left, mid]` respects the invariant \(`nums[left - 1] < nums[left]`-&gt; part of the invariant in the previous loop iteration and `nums[mid] > nums[mid + 1]` -&gt; part of the cond\)

As a result, the invariant gets reestablished. Since our termination condition is `lo < hi`, this means our loop will exit on `lo == hi`, or when the search space has been reduced to one element. That element is our answer.

