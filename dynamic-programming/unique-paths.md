#### Unique Paths:

> A robot is located at the top-left corner of a _m_ x _n_ grid \(marked 'Start' in the diagram below\).
>
> The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid \(marked 'Finish' in the diagram below\).
>
> How many possible unique paths are there?
>
> ![](https://assets.leetcode.com/uploads/2018/10/22/robot_maze.png)  
> Above is a 7 x 3 grid. How many possible unique paths are there?
>
> **Note: **_m_ and _n_ will be at most 100.
>
> **Example 1:**
>
> ```
> Input: m = 3, n = 2
>
> Output: 3
>
> Explanation:
>
> From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:
> 1. Right -> Right -> Down
> 2. Right -> Down -> Right
> 3. Down -> Right -> Right
> ```

##### Dynamic Programming:

```py
def uniquePaths(m, n):

    grid = [[0] * n for _ in range(m)]

    for i in range(m):
        grid[i][0] = 1

    for i in range(n):
        grid[0][i] = 1

    # perform dp traversal
    for i in range(1, m):
        for j in range(1, n):
            grid[i][j] = grid[i-1][j] + grid[i][j-1]

    return grid[-1][-1]
```

We could recursively solve the problem by repeatedly doing DFS on each cell, but that will very quickly time out. The subproblem relationship comes from the realization that each square at `grid[i][j]` has at most two origin points: `grid[i-1][j]` and `grid[i][j-1]`. Therefore, to figure out how many ways there are to get to `grid[i][j]`, we need to know how many ways there are to get to grid\[i-1\]\[j\] and grid\[i\]\[j-1\]. Once this relationship is derived, the problem becomes mechanical in nature. Running time is $$\small O(m*n)$$, where $$\small m,n$$ are the dimensions of the input array.

