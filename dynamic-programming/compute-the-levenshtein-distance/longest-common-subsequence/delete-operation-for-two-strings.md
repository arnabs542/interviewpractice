#### Delete Operation for Two Strings

> Given two words word1 and word2, find the minimum number of steps required to make word1 and word2 the same, where in each step you can delete one character in either string.
>
> **Example 1:**  
>
>
> ```
> Input: "sea", "eat"
>
> Output: 2
>
> Explanation: You need one step to make "sea" to "ea" and another step to make "eat" to "ea".
> ```
>
> **Note:**
>
> 1. The length of given words won't exceed 500.
> 2. Characters in given words can only be lower-case letters.

##### Dynamic Programming:

```py
def minDistance(word1: str, word2: str) -> int:
    if not word1 or not word2:
        return abs(len(word1) - len(word2))

    # Setting up DP array
    dp = [[0] * (len(word2) + 1) for _ in range(len(word1) + 1)]

    for i in range(1, len(dp)):
        for j in range(1, len(dp[0])):
            if word1[i-1] == word2[j-1]:
                dp[i][j] = 1 + dp[i-1][j-1]
            else:
                dp[i][j] = max(dp[i-1][j], dp[i][j-1])
    
    return len(word1) + len(word2) - 2*dp[-1][-1] 
```

This problem is more of an exercise in pattern recognition than anything else. If we're able to find the longest common subsequence of the two words, then the total number of delete operations we need to perform is simply the sum of the differences between each word and the longest common subsequence. Running time and space complexity don't change.

