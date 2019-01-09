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

##### Brute Force \(DFS\):

```py
def isInterleave(s1, s2, s3):
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



