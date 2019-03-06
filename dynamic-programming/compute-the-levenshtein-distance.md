#### Compute the Levenshtein Distance

> Given two words _word1_ and _word2_, find the minimum number of operations required to convert _word1_ to _word2_.
>
> You have the following 3 operations permitted on a word:
>
> 1. Insert a character
> 2. Delete a character
> 3. Replace a character
>
> **Example 1:**
>
> ```
> Input: word1 = "horse", word2 = "ros"
>
> Output: 3
>
> Explanation: 
> horse -> rorse (replace 'h' with 'r')
> rorse -> rose (remove 'r')
> rose -> ros (remove 'e')
> ```
>
> **Example 2:**
>
> ```
> Input: word1 = "intention", word2 = "execution"
>
> Output: 5
>
> Explanation: 
> intention -> inention (remove 't')
> inention -> enention (replace 'i' with 'e')
> enention -> exention (replace 'n' with 'x')
> exention -> exection (replace 'n' with 'c')
> exection -> execution (insert 'u')
> ```

##### Dynamic Programming:

```py
def minDistance(word1: str, word2: str) -> int:
    dp = [[0] * (len(word2) + 1) for _ in range(len(word1) + 1)]
    
    for i in range(len(dp)):
        dp[i][0] = i
    
    for i in range(len(dp[0])):
        dp[0][i] = i
        
    for i in range(1, len(dp)):
        for j in range(1, len(dp[0])):
            if word1[i-1] == word2[j-1]:
                dp[i][j] = dp[i-1][j-1]
            else:
                dp[i][j] = min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1]) + 1
        
    return dp[-1][-1]
```



