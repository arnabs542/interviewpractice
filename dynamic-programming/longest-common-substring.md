#### Longest Common Substring

> Write a program that computes the length of the longest common subsequence of three given strings. For example, given "epidemiologist", "refrigeration", and "supercalifragilisticexpialodocious", it should return `5`, since the longest common subsequence is "eieio".

This problem is a derivative of the LCS problem that asks for the longest common subsequence of two given strings. For simplicity, I will solve the classic problem.

##### Dynamic Programming:

```py
def longestCommonSubsequence(a, b, c):

    # Calculate longest subsequence
    dp = [[0] * (len(a) + 1) for _ in range(len(b) + 1)]
    for i in range(1, len(dp)):
        for j in range(1, len(dp[0])):
            if a[j-1] != b[i-1]:
                dp[i][j] = max(dp[i-1][j], dp[i][j-1])
            else:
                dp[i][j] = max(dp[i][j], 1 + dp[i-1][j-1])

    # Recover any of the longest subsequences
    i, j = len(dp) - 1, len(dp[0]) - 1
    size = dp[-1][-1]
    res = []

    while size > 0:
        # Current element is a match
        if dp[i][j] != dp[i-1][j] and dp[i][j] != dp[i][j-1]:
            res.append(b[i-1])
            i -= 1
            j -= 1
            size -= 1
        else:
            if dp[i][j] == dp[i-1][j]:
                i -= 1
            else:
                j -= 1

    return res[::-1]
```

The idea is to use dynamic programming to perform incremental comparisons between the strings. The entry at `dp[i][j]` records the longest common substring between `b[:i]` and `a[:j]` \(inclusive\). If the characters at indices `i,j` don't match, then it means the new character introduced doesn't help us increase our LCS, so we simply take the best of what we had before: `dp[i][j] = max(dp[i-1][j], dp[i][j-1])`. However, if the characters do match, then we can either use the new characters to build a LCS, or keep what we had before.

We can rebuild a LCS by starting from the end of our matrix, and working backwards depending on how the current entry got updated. 

To extend this to three strings, we simply make our matrix three-dimensional and check `a[i], b[j], c[k]` and update the array the same way as before.

