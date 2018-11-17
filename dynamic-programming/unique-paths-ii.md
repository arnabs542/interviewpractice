#### Unique Paths II

> A robot is located at the top-left corner of a _m_ x _n_ grid \(marked 'Start' in the diagram below\).
>
> The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid \(marked 'Finish' in the diagram below\).
>
> Now consider if some obstacles are added to the grids. How many unique paths would there be?
>
> ![](https://assets.leetcode.com/uploads/2018/10/22/robot_maze.png)
>
> An obstacle and empty space is marked as `1` and `0` respectively in the grid.
>
> **Note: **_m_ and _n_ will be at most 100.
>
> **Example 1:**
>
> ```
> Input:
> [
>   [0,0,0],
>   [0,1,0],
>   [0,0,0]
> ]
>
> Output: 2
>
> Explanation:
>
> There is one obstacle in the middle of the 3x3 grid above.
> There are two ways to reach the bottom-right corner:
> 1. Right -> Right -> Down -> Down
> 2. Down -> Down -> Right -> Right
> ```

##### Dynamic Programming:

```py
def uniquePathsWithObstacles(obstacleGrid):

    # change all obstacles to -1
    for i in range(len(obstacleGrid)):
        for j in range(len(obstacleGrid[0])):
            obstacleGrid[i][j] = -1 if obstacleGrid[i][j] == 1 else obstacleGrid[i][j]               


    # initial setup
    if obstacleGrid[0][0] == -1:
        return 0    # starting position is blocked
    else:
        obstacleGrid[0][0] = 1

    for i in range(1, len(obstacleGrid)):
        if obstacleGrid[i][0] == -1:
            obstacleGrid[i][0] = 0
        else:
            obstacleGrid[i][0] = obstacleGrid[i-1][0]

    for i in range(1, len(obstacleGrid[0])):
        if obstacleGrid[0][i] == -1:
            obstacleGrid[0][i] = 0
        else:
            obstacleGrid[0][i] = obstacleGrid[0][i-1]

    # perform dp traversal
    for i in range(1, len(obstacleGrid)):
        for j in range(1, len(obstacleGrid[0])):
            if obstacleGrid[i][j] == -1:
                obstacleGrid[i][j] = 0
            else:
                obstacleGrid[i][j] = obstacleGrid[i-1][j] + obstacleGrid[i][j-1]

    return obstacleGrid[-1][-1]
```

We could recursively solve the problem by repeatedly doing DFS on each cell, but that will very quickly time out. The subproblem relationship comes from the realization that each square at `grid[i][j]` has at most two origin points: `grid[i-1][j]` and `grid[i][j-1]`. Therefore, to figure out how many ways there are to get to `grid[i][j]`, we need to know how many ways there are to get to grid\[i-1\]\[j\] and grid\[i\]\[j-1\]. Once this relationship is derived, the problem becomes mechanical in nature. The above solution uses the input array to compute the final answer, requiring two full traversals of the array. If we initialize a new array, we can reduce time at the expense of memory. Running time is $$\small O(m*n)$$, where $$\small m,n$$ are the dimensions of the input array. 

