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

##### Solution:

The first instict when we are told find the `k`th largest or `k` smallest/largest elements should be to consider a heap. In this situation, we would find every pair, push them into the heap, prune the heap as necessary, and the return the largest element in the heap at the end:

```py
def smallestDistancePair(nums, k):
    
    max_heap = []
    n = len(nums)
    
    # Calculate all distances between pair and put them into maxheap of size k
    for i in range(n):
        for j in range(i+1, n):
            dist = abs(nums[i] - nums[j])
            heapq.heappush(max_heap, -dist)
            if len(max_heap) == k + 1:
                heapq.heappop(max_heap)
                
    return -max_heap[0]
```

Getting all pairs takes $\small \mathcal O(n^{2})$ time, and each element could be pushed and popped once from the heap, resulting in a time of $\small \mathcal O(n^{2} \log{k})$, bringing our total time to $\small \mathcal O(n^{2} + n^{2} \log{k})$. The main issue with this approach is that we're actually doing more work than required - we're not just finding the `k`th smallest distances, but we're actually finding all `k` smallest distances.

This problem is not immediately clear that it's a trial-and-error type of problem. However, the maximum pair distance is actually bounded by `max(nums) - min(nums)`. Instead of finding the `k`th smallest distance, let's just guess a distance in our search space, and see if there are exactly `k-1` elements smaller than/equal to it.

```py
def smallestDistancePair(nums, k):
    def possible(guess):
        #Is there k or more pairs with distance <= guess?
        count = left = 0
        for right, x in enumerate(nums):
            while x - nums[left] > guess:
                left += 1
            count += right - left
        return count >= k

    nums.sort()
    lo = 0
    hi = nums[-1] - nums[0]
    while lo < hi:
        mi = (lo + hi) / 2
        if possible(mi):
            hi = mi
        else:
            lo = mi + 1

    return lo
```

##### Explanation:

This problem is a little harder to understand.

We first figure out the search space, which is `[0, max(nums) - min(nums)]`. We define the K-th smallest pair distance: given an integer `dist`, let `count(dist)` denote the number of pair distances that are no greater than `dist`, then the K-th smallest pair distance will be the smallest integer such that `count(dist) >= K`. This is the verification portion.

The trial section of the code is relatively straight forward - shrink the search space until we converge on the smallest `num` which causes `countLTE(nums, num)` to return True.

Naïvely implemented, the verification portion would take $\small \mathcal O(n^{2})$ time, since we'd have to loop through all pairs and see how many them are less than num. However, doing it like so disregards the previous work we put into sorting the array. We will use a sliding window approach to count the number of pairs with distance `<=` guess. For every possible `right`, we maintain the loop invariant: `left` is the smallest value such that `nums[right] - nums[left] <= guess`. Then, the number of pairs with `right` as it's right-most endpoint is `right - left`, and we add all of these up.

 Time Complexity: $\small O(N \log{W} + N \log{N})$, where $\small N$ is the length of `nums`, and $\small W$ is equal to `nums[-1] - nums[0]`. The $\small \log{W}$ factor comes from our binary search, and we do $\small \mathcal O(n)$ work each inside each call for verification. The $\small \mathcal O(N \log{N})$ factor comes from the initial sorting of the array. 



