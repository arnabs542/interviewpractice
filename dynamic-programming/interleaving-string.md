#### Interleaving String:

> Given _s1_, _s2_, _s3_, find whether _s3_ is formed by the interleaving of _s1_ and _s2_.
>
> **Example 1:**
>
> ```
> Input: s1 = "aabcc", s2 = "dbbca", s3 = "aadbbcbcac"
>
> Output: true
> ```
>
> **Example 2:**
>
> ```
> Input: s1 = "aabcc", s2 = "dbbca", s3 = "aadbbbaccc"
>
> Output: false
> ```

##### Brute Force:

```
def isInterleave(self, s1, s2, s3):
    """
    :type s1: str
    :type s2: str
    :type s3: str
    :rtype: bool
    """

    def helper(s1_idx, s2_idx, s3_idx):
        if s3_idx == len(s3):
            return s1_idx == len(s1) and s2_idx == len(s2)
        if s2_idx < len(s2) and s3[s3_idx] == s2[s2_idx]:
            if helper(s1_idx, s2_idx + 1, s3_idx + 1):
                return True
        if s1_idx < len(s1) and s3[s3_idx] == s1[s1_idx]:
            if helper(s1_idx + 1, s2_idx, s3_idx + 1):
                return True
        return False

    return helper(0, 0, 0)
```

The brute force solution is to simply try all combinations of rebuilding the string. This leads to a runtime of $$\small O((m+n)!)$$ where $$\small m,n$$ represent the lengths of $$\small s1, s2$$.

##### Dynamic Programming:

```py
def isInterleave(s1, s2, s3):

    if len(s1) + len(s2) != len(s3):
        return False

    if not s1:
        return s2 == s3

    if not s2:
        return s1 == s3

    dp = [[False] * ((len(s2)) + 1) for _ in range(len(s1)+1)]

    for i in range(len(dp)):
        for j in range(len(dp[0])):
            if i == j == 0:
                dp[i][j] = True
            else:
                dp[i][j] = ((s3[i+j-1] == s2[j-1]) and dp[i][j-1]) or ((s3[i+j-1] == s1[i-1]) and dp[i-1][j])

    return dp[-1][-1]
```

The DP table represents if `s3[:(i+j)]` can be built by interleaving `s1[:i]` and `s2[:j]`. The 0th position represents an empty string for all three strings, which is trivially true. Whenever we introduce a new character to `s3`, we check if that matches either the last character in `s1` or `s2`, and if so, if we could already built `s3` without the matching character. If yes, then we can build `s3` with the new character as well. Otherwise, `s1` and `s2` cannot currently be interleaved to build `s3`.

Runtime and space are both bounded by $$\small \mathcal O(m*n)$$. Again, always be careful when accessing the DP array, since the array will usually have an extra position than the input as a base case. If we did not include the three edge case checks before the DP solution, we would encounter an error for an input like `s1 = "a", s2 = "", s3 = "a"`. At `j = 0`, we'll try to access `s2[j-1]`, which will thrown an index out of range error, unless we add additional checks when attempting to access.

