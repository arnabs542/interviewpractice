#### Number of Strict Monotone Sequences

> Call a decimal number $$\small D$$, as defined above, strictly monotone if $$\small D[i] < D[i+1], 0 \leq i < |{D}|$$. Write a program which takes as input a positive integer $$\small k$$ and computes the number of decimal numbers of length $$\small k$$ that are strictly monotone.

##### Recursion -&gt; Dynamic Programming:

```py
def monotoneSequence(k):

    def helper(k, nums):
        if k == 0 or not nums:
            return 0
        if k == 1:
            return nums
        res = 0
        for i in range(nums):
            res += helper(k-1, nums - i - 1)
        return res

    return helper(k, 9)
```

The only difference between this and the parent problem is for the recursive call, we have to pass in `nums-i-1` since we can't use the same digit again. Otherwise completely the same.

Again, we can either use top-down:

```py
def monotoneSequence(k):
    memo = {}
    def helper(k, nums):
        key = (k, nums)
        if key in memo:
            return memo[key]
        if k == 0 or not nums:
            memo[key] = 0
            return memo[key]
        if k == 1:
            memo[key] = nums
            return memo[key]
        res = 0
        for i in range(nums):
            res += helper(k-1, nums - i - 1)
        memo[key] = res
        return memo[key]

    return helper(k, 9)
```

or bottom-up:

```py
def monotoneSequence(k):
    nums = 9
    dp = [[0] * (nums+1) for _ in range(k+1)]

    for i in range(nums+1):
        dp[1][i] = i

    for i in range(2, k+1):
        for j in range(i, nums+1):
            dp[i][j] += dp[i-1][j-1] + dp[i][j-1]

    return dp[-1][-1]
```

The bottom up idea is as follows: suppose we introduce a new number. We can either choose to not use it \(`dp[i][j-1]`\). If we wish to use the new number, we can attach it to the end of all the sequences of length `i-1` that didn't use the number \(`dp[i-1][j-1]`\).

