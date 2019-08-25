#### Shortest Common Supersequence

> Given two strings `str1` and `str2`, return the shortest string that has both `str1` and `str2` as subsequences.  If multiple answers exist, you may return any of them.
>
> (A string `S` is a subsequence of string `T` if deleting some number of characters from `T` (possibly `0`, and the characters are chosen anywhere from `T`) results in the string `S`.)
>
> **Example 1**:
>```
> Input: str1 = "abac", str2 = "cab"
> Output: "cabac"
> ```
> Explanation:
> ``` 
> str1 = "abac" is a substring of "cabac" because we can delete the first "c".
> str2 = "cab" is a substring of "cabac" because we can delete the last "ac".
>```
>The answer provided is the shortest such string that satisfies these properties.
>
>Note:
>
> * 1 <= `str1.length`, `str2.length` <= 1000
> * str1 and str2 consist of lowercase English letters.

##### Dynamic Programming

```py
def shortestCommonSupersequence(str1: str, str2: str) -> str:
    
    # Find LCS first    
    def find_lcs(str1, str2):
        n, m = len(str1), len(str2)
        lcs = [["" for _ in range(m+1)] for _ in range(n+1)]

        for i in range(n):
            for j in range(m):
                if str1[i] == str2[j]:
                    lcs[i+1][j+1] = lcs[i][j] + str1[i]
                else:
                    lcs[i+1][j+1] = max(lcs[i+1][j], lcs[i][j+1], key=len)

        return lcs[-1][-1]
    
    res = []
    i = j = 0
    n, m = len(str1), len(str2)
    
    for c in find_lcs(str1, str2):
        while i < n and str1[i] != c:
            res.append(str1[i])
            i += 1
        while j < m and str2[j] != c:
            res.append(str2[j])
            j += 1
        res.append(c)
        i, j = i+1, j+1
    
    return "".join(res) + str1[i:] + str2[j:]
```

The key trick to this problem was realizing that we could reframe it as a an LCS problem. By using the LCS as the "core" of our result, our result is just the concatenation of all characters from both strings not in the LCS with the LCS.

Time used is dominated by the DP array - $\small \mathcal O(nm)$ time. We used $\small \mathcal O(nm) * O(string)$ for the space, but we can just store numbers in the DP array and rebuild the LCS saving us the space.