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

The brute force solution would be enumerate all strings that are distance 1,2,3... from the first string, and see how long it takes us to reach the second string. The number of strings may grow enormously, e.g., if the first string is $$\small n$$ 0s and the second string is $$\small n$$ 1s we will visit all $$\small 2^{n}$$possible bit strings from the first string before we reach the second string. 

The subproblem recurrence is this: suppose we know the Levenshtein Distance between `word1[:-1]` and `word2[:-1]`. There are two cases:

1. If `word1[-1] == word2[-1]`, then the Levenshtein Distance is the same as the distance between `word1[:-1]` and `word2[:-1]`. This is because the Levenshtein Distance is the amount of transformations required to make `word1[:-1] == word2[:-1]`,  so tacking on two similar letters doesn't change it.
2. If `word1[-1] != word2[-1]` , then we have 3 options:
   1. Delete the last character from `word2` and add 1 to `dist(word1, word2[:-1])`
   2. Delete the last character from `word1` and add 1 to the `dist(word1[:-1], word2)`
   3. Get `dist(word1[:-1], word2[:-1])` and then add 1, signifying appending either `word1[-1]` or `word2[-1]` to the resulting transformation of `word1[:-1]` to `word2[:-1]`

Let $$\small a,b$$ be the lengths of the two strings. The above solution runs in $$\small \mathcal O(ab)$$ time and uses $$\small \mathcal O(ab)$$ space. In terms of time complexity, we can't really improve too much. However, we can improve space usage dramatically because we only need the current and previous rows in our dp table. Below is the space optimized version of the above solution:

```py
def minDistance(word1: str, word2: str) -> int:
    
    if len(word1) < len(word2):
        word1, word2 = word2, word1

    prev_row = [i for i in range(len(word2) + 1)]
    cur_row = [1] + [0] * len(word2) 

    for i in range(1, len(word1) + 1):
        for j in range(1, len(cur_row)):
            if word1[i-1] == word2[j-1]:
                cur_row[j] = prev_row[j-1]
            else:
                cur_row[j] = min(cur_row[j-1], prev_row[j], prev_row[j-1]) + 1

        prev_row = cur_row
        cur_row = [prev_row[0] + 1] + [0] * len(word2)

    return prev_row[-1]
```



