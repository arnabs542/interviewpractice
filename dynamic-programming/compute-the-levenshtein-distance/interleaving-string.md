#### Interleaving String

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

##### Brute Force \(TLE\):

```
def isInterleave(s1: str, s2: str, s3: str) -> bool:
    
    def helper(s3_idx, s1_idx, s2_idx):
        if s3_idx == len(s3):
            return s1_idx == len(s1) and s2_idx == len(s2)
        if s1_idx < len(s1) and s3[s3_idx] == s1[s1_idx]:
            if helper(s3_idx + 1, s1_idx + 1, s2_idx):
                return True
        if s2_idx < len(s2) and s3[s3_idx] == s2[s2_idx]:
            if helper(s3_idx + 1, s1_idx, s2_idx + 1):
                return True
        return False
    
    return helper(0, 0, 0)
```

The brute force solution is to simply try every combination. We being to work on `s3` one character at a time. If the current `s3` character matches the next `s1` character, we see if using that lets us build the string. If not, we see if it matches the next `s2 `character. Our base case is when we finish building all characters in `s3`; we then check to make sure we used all of `s1` and `s2`.

Let $$\small n, m$$ be the length of `s1` and `s2`. The runtime is bounded by $$\small \mathcal O((n*m)!)$$.

##### Dynamic Programming \(Top-down\):

```py
def isInterleave(s1: str, s2: str, s3: str) -> bool:
    
    memo = {}
    
    def helper(s3_idx, s1_idx, s2_idx):
        key = (s3_idx, s1_idx, s2_idx)
        if key in memo:
            return memo[key]
        if s3_idx == len(s3):
            memo[key] = s1_idx == len(s1) and s2_idx == len(s2)
            return memo[key]
        if s1_idx < len(s1) and s3[s3_idx] == s1[s1_idx]:
            if helper(s3_idx + 1, s1_idx + 1, s2_idx):
                memo[key] = True
                return memo[key]
        if s2_idx < len(s2) and s3[s3_idx] == s2[s2_idx]:
            if helper(s3_idx + 1, s1_idx, s2_idx + 1):
                memo[key] = True
                return memo[key]
        memo[key] = False
        return False
    
    return helper(0, 0, 0)
```

We can simply memoize all of our previous work to reduce time complexity.

