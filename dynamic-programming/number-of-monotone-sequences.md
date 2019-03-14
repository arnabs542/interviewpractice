#### Number of Monotone Sequences

> A decimal number is a sequence of digits, i.e., a sequence over$$\small \{0,1,2,...,9\}$$. The sequence has to be of length 1 or more, and the first element in the sequence cannot be 0. Call a decimal number D monotone if $$\small D[i] < D[i+1], 0 \leq i < |{D}|$$. Write a program which takes as input a positive integer $$\small k$$ and computes the number of decimal numbers of length $$\small k$$ that are monotone.

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
            res += helper(k-1, nums - i)
        return res

    return helper(k, 9)
```

We're told that a sequence cannot start with 0; since 0 is smaller than any other digit, we essentially only consider the digits 1 - 9.

If $$\small k = 1$$, then we simply return the number of digits we haven't used it. Otherwise, we loop through the number of unused digits, pick one to start the sequence, and see how many sequences of length $$\small k-1$$ we can make with the rest of the digits that are equal to or greater than the digit we just chose.

We can anticipate a lot of repeating subproblems - thus, we can simply cache our previous results to speed up computation:

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
            res += helper(k-1, nums - i)
        memo[key] = res
        return memo[key]

    return helper(k, 9)
```

We can also solve this bottom up; Let `dp` be our dynamic programming array, and let `dp[i][j]` hold the number of length `i` monotone sequences we can construct with `j` numbers.

```py
def monotoneSequence(k):
    nums = 9
    dp = [[0] * (nums+1) for _ in range(k+1)]

    for i in range(nums+1):
        dp[1][i] = i

    for i in range(2, k+1):
        for j in range(1, nums+1):
            dp[i][j] += dp[i-1][j] + dp[i][j-1]

    return dp[-1][-1]
```

When we introduce a new number, we can add it to the end all the sequences without it \(`dp[i][j-1]`\), and we can add it to the end of all the sequences that are one number shorter \(`dp[i-1][j]`\).

Runtime and space are bounded by $$\small \mathcal O(k*n)$$, but since $$\small \mathcal n$$ is bounded at 9, the bounds simplify to $$\small \mathcal O(n)$$.

