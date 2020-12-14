#### Partition Equal Subset Sum

> Given a **non-empty** array containing **only positive integers**, find if the array can be partitioned into two subsets such that the sum of elements in both subsets is equal.
>
> **Note:**
>
> 1. Each of the array element will not exceed 100.
> 2. The array size will not exceed 200.
>
> **Example 1:**
>
> ```
> Input: [1, 5, 11, 5]
>
> Output: true
>
> Explanation: The array can be partitioned as [1, 5, 5] and [11].
> ```
>
> **Example 2:**
>
> ```
> Input: [1, 2, 3, 5]
>
> Output: false
>
> Explanation: The array cannot be partitioned into equal sum subsets.
> ```

##### Dynamic Programming:

The brute force solution would be to try all possible partitions of the elements. Since each element can go into one of two subsets, this results in a time complexity of $\small \mathcal O(2^{n})$ time complexity. Instead, we cache previous work to save on time.:

```py
from functools import lru_cache

def canPartition(nums: List[int]) -> bool:

    if sum(nums) % 2:
        return False

    @lru_cache(None)
    def helper(left, right, idx):
        if idx >= len(nums):
            return left == right
        if helper(left + nums[idx], right, idx+1):
            return True
        return helper(left, right + nums[idx], idx+1)

    return helper(0, 0, 0)
```

I'm using the `lru\_cache` decorator from `functools`, but we can easily convert that to a manual dictionary using `(left, right, idx)` as the key.

In reality, this problem is a disguised version of the knapsack problem. The bag capacity is `sum(nums) / 2`, and we need to pick elements to try to hit that target. Interestingly enough, the bottom-up solution actually times out:

```py
def canPartition(nums: List[int]) -> bool:
    target = sum(nums)
    if target % 2:
        return False

    target >>= 1
    dp = [[0] * (target + 1) for _ in range(len(nums))]
    for i in range(len(nums)):
        for j in range(len(dp[0])):
            if j >= nums[i]:
                dp[i][j] = max(dp[i][j], dp[i-1][j - nums[i]] + nums[i])
            dp[i][j] = max(dp[i][j], dp[i-1][j])

    return dp[-1][-1] == target
```

The quickest solution is a modified version of the top-down solution above. Instead of using two variables to keep track of the sum of elements in the left and right subsets, we try to reduce a single target variable to 0:

```py
def canPartition(nums: List[int]) -> bool:

    def canfind(idx, target, memo):
        if target in memo:
            return memo[target]
        if target == 0:
            memo[target] = True
        else:
            memo[target] = False
            if target > 0:
                for i in range(idx, len(nums)):
                    if canfind(i+1, target - nums[i], memo):
                        memo[target] = True
                        break
        return memo[target]

    s = sum(nums)
    if s % 2:
        return False

    return canfind(0, s // 2, {})
```

The bottom up solution actually requires only a 1D array; the solution is actually more similar to the coin problem, which itself is a type of the 0/1 knapsack problem:

```py
def canPartition(nums: List[int]) -> bool:
    target = sum(nums)
    if target & 1:
        return False
    target >>= 1
    dp = [False] * (target + 1)
    dp[0] = True
    for c in sorted(nums):
        for i in range(target, c-1, -1):
            dp[i] = dp[i] or dp[i-c]
        if dp[-1]:
            return True
    return False
```



