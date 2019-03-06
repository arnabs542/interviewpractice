#### Longest Common Subsequence

> Given two strings $$\small A$$ and $$\small B$$, compute a longest sequence of characters that is a subsequence of $$\small A$$ and $$\small B$$. For example, the longest subsequence present in "Carthorse" and "Orchestra" is $$\small <r, h, s>$$.

##### Dynamic Programming:

```py
def longestCommonSubsequence(A, B):
    # Edge case check
    if not A or not B:
        return 0
            
    # Setting up DP array
    dp = [[0] * (len(B) + 1) for _ in range(len(A) + 1)]
    
    for i in range(1, len(dp)):
        for j in range(1, len(dp[0])):
            if A[i-1] == B[j-1]:
                dp[i][j] = 1 + dp[i-1][j-1]
            else:
                dp[i][j] = max(dp[i-1][j], dp[i][j-1])
                
    # Rebuild the subsequence
    subsequence = []
    i, j = len(dp) - 1, len(dp[0]) - 1
    left = dp[-1][-1]
    
    while left:
        if A[i-1] == B[j-1]:
            subsequence.append(A[i-1])
            left -= 1
            i -= 1
            j -= 1
        elif dp[i][j] == dp[i-1][j]:
            i -= 1
        else:
            j -= 1
    
    return subsequence[::-1]
```

This problem is essentially the same idea as the parent problem. We begin by assuming we know the longest common subsequence 

