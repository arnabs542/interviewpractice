#### Count Ways with Obstacles

> Solve the same problem in the presence of obstacles, specified by a Boolean 2D array, where the presence of a true value represents an obstacle.

##### Dynamic Programming:

```py
def uniquePathsWithObstacles(obstacleGrid: "List[List[int]]") -> int:
    
    if not obstacleGrid or not obstacleGrid[0] or obstacleGrid[0][0] == 1:
        return 0
    
    n,m = len(obstacleGrid), len(obstacleGrid[0])
    dp = [[0] * m for _ in range(n)]
    
    for i in range(m):
        dp[0][i] = (1 - obstacleGrid[0][i]) & (dp[0][i-1] if i > 0 else 1)
    for i in range(n):
        dp[i][0] = (1 - obstacleGrid[i][0]) & (dp[i-1][0] if i > 0 else 1)
    
    for i in range(1, n):
        for j in range(1, m):
            if obstacleGrid[i][j] == 1:
                dp[i][j] = 0
            else:
                dp[i][j] = dp[i-1][j] + dp[i][j-1]
    
    return dp[-1][-1]        
```

The only change here is that when we traverse our dp array, we have to check if the original value has an obstacle. If so, then that cell has a value of 0, instead of the sum of the two neighboring cells.

Space can be optimized by keeping only the two rows that we need.

