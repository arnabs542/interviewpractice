#### Find K-th Smallest Pair Distance

> Given an integer array, return the k-th smallest **distance** among all the pairs. The distance of a pair \(A, B\) is defined as the absolute difference between A and B.
>
> **Example 1:**
>
> ```
> Input:
>
> nums = [1,3,1]
> k = 1
>
> Output: 0
> Explanation:
>
> Here are all the pairs: 
> (1,3) -> 2 
> (1,1) -> 0
> (3,1) -> 2
>
> Then the 1st smallest distance pair is (1,1), and its distance is 0.
> ```
>
> **Note:**
>
> 1. `2 <= len(nums) <= 10000`
> 2. `0 <= nums[i] < 1000000`
> 3. `1 <= k <= len(nums) * (len(nums) - 1) / 2`

##### Code:

```py
def smallestDistancePair(self, nums, k):

    nums.sort()

    def countLTE(nums, dist):
        ans = 0
        for i in range(len(nums)):
            ans += bisect.bisect_right(nums, nums[i] + dist) - i - 1
        return ans

    lo, hi = 0, nums[-1] - nums[0]
    while lo < hi:
        mid = (lo + hi) >> 1
        if countLTE(nums, mid) < k:
            lo = mid + 1
        else:
             hi = mid
    return lo
```

##### Explanation:

This problem is a little harder to understand.

We first figure out the search space, which is `[0, max(nums) - min(nums)]`. We define the K-th smallest pair distance: given an integer `dist`, let `count(dist)` denote the number of pair distances that are no greater than `dist`, then the K-th smallest pair distance will be the smallest integer such that `count(dist) >= K`. This is the verification portion.

The trial section of the code is relatively straight forward - shrink the search space until we converge on the smallest `num` which causes `countLTE(nums, num)` to return True.

Naïvely implemented, the verification portion would take $$\small \mathcal O(n^{2})$$ time, since we'd have to loop through all pairs and see how many them are less than num. However, doing it like so disregards the previous work we put into sorting the array. Let us denote interval `d_i`j as an interval that begins at `nums[i]` and ends at `nums[j]`. If we hold `i` steady and increase `j` until `d_ij > nums[i] + dist`, then we've found all intervals beginning at `i` that has a distance less than num. Since the array is sorted, and we're essentially looking for the right most insertion point of `nums[i] + dist`, we can also use binary search to assist in the verification portion. In fact, this is a bit similar to the one-armed quicksort algorithm to find the median, where we discard the array based on how many elements lie to the left of a certain element.

I think the overall runtime complexity is: $$\small \mathcal O(\log(d) * n * \log(n))$$.

The initial sort takes $$\small \mathcal O(n \log(n))$$ time. The trial takes $$\small \mathcal O(\log(d))$$ time, where `d = nums[-1] - nums[0]`. During each trial, the verification takes $$\small \mathcal O(n \log(n))$$.

