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

The idea behind the above solution is that we will build a 2d memo `dp` where `dp[j][i]` records the number of times the subsequence `t[0:j]` is included in `s[0:i]`.  Therefore the result will be `dp[T.length()][S.length()]`. we can build this array rows-by-rows:

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

* If the current character in S doesn't equal to current character T, then we have the same number of distinct subsequences as we had without the new character, since it contributes nothing.
* If the current character in S equal to the current character T, then the distinct number of subsequences: the number we had before **plus** the distinct number of subsequences we had with less longer T and less longer S. In other words, we need to count the contribution with the new character and without.

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



