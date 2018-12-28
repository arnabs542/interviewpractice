#### Largest Divisible Subset

> Given a set of **distinct **positive integers, find the largest subset such that every pair \(Si, Sj\) of elements in this subset satisfies:
>
> Si% Sj= 0 or Sj% Si= 0.
>
> If there are multiple solutions, return any subset is fine.
>
> **Example 1:**
>
> ```
> Input: [1,2,3]
> Output: [1,2] (of course, [1,3] will also be ok)
> ```
>
> **Example 2:**
>
> ```
> Input: [1,2,4,8]
> Output: [1,2,4,8]
> ```

I initially thought this was a DSU type problem, but the difficulty in using a DSU would be figuring what a common root is for each group, since a number could be assigned to multiple subsets.

In reality, this is another disguised LIS problem. Suppose we have a subset $$\small S$$ which satisfies the condition. A new element $$\small E$$ would only be included in the subset if $$\small E \> \% {} \> max(S) == 0$$ or $$\small E \> \% \> min(S) == 0$$. If we sort the array, then we would only get increasing elements, so we only need to check the first condition.

##### Dynamic Programming:

```py
def largestDivisibleSubset(nums):

    n = len(nums)
    if not n:
        return []

    nums.sort()
    dp, pre_idx = [1]*n, [0]*n
    max_lens = max_idx = 0

    for i in range(n):
        for j in range(i):
            # If we find a larger subset, append nums[i] to end of subset
            if nums[i] % nums[j] == 0 and dp[j] + 1 > dp[i]:
                dp[i] = dp[j] + 1
                pre_idx[i] = j  # Remember the element before nums[i]
        if dp[i] > max_lens:
            max_lens = dp[i]
            max_idx = i

    # Work backwards from last element to rebuild subset
    res = [nums[max_idx]]
    for i in range(max_lens - 1):
        max_idx = pre_idx[max_idx]
        res.append(num[max_idx])
    return res
```



