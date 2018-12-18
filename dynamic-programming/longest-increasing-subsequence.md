#### Longest Increasing Subsequence

> Given an unsorted array of integers, find the length of longest increasing subsequence.
>
> **Example:**
>
> ```
> Input:[10,9,2,5,3,7,101,18]
>
> Output: 4 
>
> Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4. 
> ```

A classic dynamic programming problem. The subproblem pattern is this: suppose we know the longest increasing subsequence for a group of $$\small n$$ elements. If we were to add another element $$\small e$$, how would that affect the current LIS?

Bottom Up:

```py
def lengthOfLIS(nums):
    
    dp = [1] * len(nums)
    
    for i in range(len(nums)):
        for j in range(i):
            if nums[i] > nums[j]:
                dp[i] = max(dp[i], dp[j] + 1)
    
    return max(dp) if dp else 0
```

This a simple brute force solution to calculate the LIS. We use a table `dp` that records the longest entry ending at each element. In other words, `dp[i]` is the LIS if we include `nums[i]`. We then run a nested for loop. If `nums[i] > nums[j]`, then `dp[i] = max(dp[i], dp[j] + 1)`. Running time is $$\small \mathcal O(n^{2})$$.

##### Binary Search:

```py
def lengthOfLIS(nums):

    chain = []

    def search(x):
        lo, hi = 0, len(chain) - 1
        while lo < hi:
            mid = (lo + hi) >> 1
            if chain[mid] < x:
                lo = mid + 1
            else:
                hi = mid
        return lo

    for e in nums:
        if not chain or e > chain[-1]:
            chain.append(e)
        else:
            idx = search(e)
            chain[idx] = e

    return len(chain)
```



