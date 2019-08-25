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

The brute force solution is a simple DFS method that attempts to build all substrings of length equal to length of `t`, then check if we succeeded in reconstructing `t`. The run time on this is bounded either exponential or factorial time, but either way, it times out and is unacceptably slow.

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
##### Top-down DP:

Let's first formulate a recursive solution. Let `dp(i, j)` return the number of subsequences in `s[i:]` that equals `t[j:]`. We have two scenarios:
1. `s[i]` == `t[j]`, in which case we can continue with `dp(i+1, j+1)`
2. We can also try `dp(i+1, j)` to match `s[i+1:]` and `t[j:]`

Our recurrence relationship is therefore:
```py
dp(i,j) = dp(i+1, j) + (dp(i+1, j+1) if s[i] == t[j] else 0)
```

Our base case is as follows:
1. if `j == len(t)`, then we're trying to match a null string to a string, and there is only `1` match
2. if `i == len(s)`, then our pattern is a null string, which can only match a null string. Return `1` if `j == len(t)`, else `0`

The top down solution is as follows:

```py
def numDistinct(s: str, t: str) -> int:
    memo = {}
    
    def helper(s_idx, t_idx):
        key = (s_idx, t_idx)
        if t_idx == len(t):
            return 1
        if s_idx == len(s):
            return 0
        if key not in memo:
            count = 0
            if s[s_idx] == t[t_idx]:
                count += helper(s_idx+1, t_idx+1)
            count += helper(s_idx+1, t_idx)
            memo[key] = count
        return memo[key]

    return helper(0, 0)
```

##### Dynamic Programming (Bottom up):

The idea behind the solution is that we will build a 2d memo `dp` where `dp[j][i]` records the number of times the subsequence `t[0:j]` is included in `s[0:i]`.  Therefore the result will be `dp[T.length()][S.length()]`. we can build this array rows-by-rows:

* The first row must be filled with 1. That's because the empty string is a subsequence of any string but only 1 time. So 
  `dp[0][i] = 1` for every `i`. The extra padding will also return correct value if `T` is an empty string.
* The first column of every rows except the first must be 0. This is because an empty string cannot contain a non-empty string as a substring -- the very first item of the array: `dp[0][0] = 1`, because an empty string contains the empty string 1 time.

So the matrix looks like this:

```
  S 0123....i
T +----------+
  |1111111111|
0 |0         |
1 |0         |
2 |0         |
. |0         |
. |0         |
j |0         |
```

From here we can easily fill the whole grid: for each `(x, y)`, we check if `S[x] == T[y]` we add the previous item and the previous item in the previous row, otherwise we copy the previous item in the same row. The logic is as follows:

* If the current character in `S` doesn't equal to current character `T`, then we have the same number of distinct subsequences as we had without the new character, since it contributes nothing.
* If the current character in `S` equal to the current character `T`, then the distinct number of subsequences: the number we had before **plus** the distinct number of subsequences we had with less longer `T` and less longer `S`. In other words, we need to count the contribution with the new character and without.

An example: `S: [acdabefbc]` and `T: [ab]`

First we check with `a`:

```
           *  *
      S = [acdabefbc]
 dp[1] = [0111222222]
```

then we check with `ab`:

```
               *  * 
      S = [acdabefbc]
 dp[1] = [0111222222]
 dp[2] = [0000022244]
```

And the result is 4, as the distinct subsequences are:

```
      S = [a b    ]
      S = [a   b  ]
      S = [  ab   ]
      S = [  a  b ]
```

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


Running time and space are both $\small \mathcal O(m*n)$, where $\small m,n$ are the lengths of the two strings. We could improve on space complexity by keeping only the two most relevant rows, since each current row only depends on the the previous row and previous column. 

