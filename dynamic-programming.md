#### Distinct Subsequences

> Given a string **S** and a string **T**, count the number of distinct subsequences of **S** which equals **T**.
>
> A subsequence of a string is a new string which is formed from the original string by deleting some \(can be none\) of the characters without disturbing the relative positions of the remaining characters. \(ie, `"ACE"` is a subsequence of `"ABCDE"` while `"AEC"` is not\).
>
> **Example 1:**
>
> ```
> Input: S = "rabbbit", T = "rabbit"
> Output: 3
>
> Explanation:
>
> As shown below, there are 3 ways you can generate "rabbit" from S.
> (The caret symbol ^ means the chosen letters)
>
> rabbbit
> ^^^^ ^^
>
> rabbbit
> ^^ ^^^^
>
> rabbbit
> ^^^ ^^^
> ```
>
> **Example 2:**
>
> ```
> Input: S = "babgbag", T = "bag"
> Output: 5
>
> Explanation:
>
> As shown below, there are 5 ways you can generate "bag" from S.
> (The caret symbol ^ means the chosen letters)
>
> babgbag
> ^^ ^
>
> babgbag
> ^^    ^
>
> babgbag
> ^    ^^
>
> babgbag
>   ^  ^^
>
> babgbag
>     ^^^
> ```

##### Brute Force:

```py
def numDistinct(s, t):

    count = [0]

    def helper(start, s, t, cur, count):            
        if len(cur) == len(t):
            count[0] += int(cur == t)
            return
        if start >= len(s):
            return
        for i in range(start, len(s)):
            if s[i] == t[len(cur)] :
                helper(i+1, s, t, cur + s[i], count)

    helper(0, s, t, "", count)
    return count[0]
```

The brute force solution is a simple DFS method that attempts to build all substrings of length equal to length of t, then check if we succeeded in reconstructing t. The run time on this is bounded either exponential or factorial time, but either way, it times out and is unacceptably slow.

##### Dynamic Programming:

```py
def numDistinct(s, t):

    dp = [[0] * (len(s) + 1) for _ in range(len(t) + 1)]
    dp[0] = [1] * (len(s) + 1)

    for i in range(1, len(dp[0])):        
        for j in range(1, len(dp)):
            if s[i-1] == t[j-1]:
                dp[j][i] = dp[j-1][i-1] + dp[j][i-1]
            else:
                dp[j][i] = dp[j][i-1]

    return dp[len(t)][len(s)]
```



