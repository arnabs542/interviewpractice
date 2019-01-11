Stone Game



DP:

```py
def stoneGame(self, piles):

    n = len(piles)
    dp= [[0] * n for _ in range(n)]
    
    for i in range(n):
        dp[i][i] = piles[i]
    
    for dist in range(1, n):
        for start in range(n - dist):
            dp[start][start+dist] = max(piles[start] - dp[start+1][start+dist], 
                                        piles[start+dist] - dp[start][start+dist-1])
    
    return dp[0][-1] > 0
```



