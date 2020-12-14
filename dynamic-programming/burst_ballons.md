#### Burst Balloons

> Given n balloons, indexed from 0 to n-1. Each balloon is painted with a number on it represented by array nums. You are asked to burst all the balloons. If the you burst balloon i you will get nums[left] * nums[i] * nums[right] coins. Here left and right are adjacent indices of i. After the burst, the left and right then becomes adjacent.

> Find the maximum coins you can collect by bursting the balloons wisely.

> Note:
>
>    You may imagine nums[-1] = nums[n] = 1. They are not real therefore you can not burst them.
>    0 ≤ n ≤ 500, 0 ≤ nums[i] ≤ 100

> Example:
```
Input: [3,1,5,8]
Output: 167 
Explanation: nums = [3,1,5,8] --> [3,5,8] -->   [3,8]   -->  [8]  --> []
             coins =  3*1*5      +  3*5*8    +  1*3*8      + 1*8*1   = 167
```

##### DP

The brute force solution would be to try every different way of popping the balloons. Starting with $n$ balloons, this leads to an $O(n!)$ solution, since at each stage, we have $n$ choices, then $n-1$, so forth. 

If we try memoization, but it leads to a time complexity of $O(C(n, k))$. At level 1, we have $C(n, n-1)$ different possiblities. At level two, we have $C(n, n-2)$, etc.

Thus, we obviously need a DP solution that is some form of $n^{k}$ or a greedy solution. A greedy solution is not immediately obvious, so we try a DP solution. 

This DP solution is a bit tricky. The key point is this:
- `dp[i][j]`: the max number of coins we can get by bursting all the balloons between `i` and `j`, **not including `i` or `j`**

We do we not include either boundary indice? Because including them doen't allow us to easily divide the subproblems. For example, suppose that `k` is some index between `i` and `j`, and also suppose that `k` is the very last ballon within that original range we pop. If we don't exclude the boundaries, then they become adjacent, which prevents us from neatly dividing the array into a left and right portion.   

Having figured out how to divide our subproblems, the recursive relationship is this:
- `dp[i][j] = max(nums[i] * nums[k] * nums[j] + dp[i][k] + dp[k][j]) (k in (i+1,j))`

In other words, if `k` is the index of the last balloon we pop in `(i, j)`, then the coins we get from popping `k` is `nums[i] * nums[k] * nums[j]`. The number of coins we get from popping all the earlier ballons in the same range is `dp[i][k] + dp[k][j]`. In essence, we work backwards from the last balloon popped instead of the first one, which is more common in dp problems.

```py
def maxCoins(nums):
    nums = [1] + [i for i in nums if i > 0] + [1]
    n = len(nums)
    dp = [[0] * n for _ in range(n)]
    
    for length in range(2, n):
        for i in range(n - length):
            j = i + length
            for k in range(i+1, j):
                dp[i][j] = max(dp[i][j], nums[i] * nums[k] * nums[j] + dp[i][k] + dp[k][j])
    
    return dp[0][-1]
    ```