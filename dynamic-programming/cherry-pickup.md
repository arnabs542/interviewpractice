#### Cherry Pickup - LC 741

> In a N x N grid representing a field of cherries, each cell is one of three possible integers.
>
>   * 0 means the cell is empty, so you can pass through;
>   * 1 means the cell contains a cherry, that you can pick up and pass through;
>   * -1 means the cell contains a thorn that blocks your way.
>
> Your task is to collect maximum number of cherries possible by following the rules below:
> * Starting at the position `(0, 0)` and reaching `(N-1, N-1)` by moving right or down through valid path cells (cells with value `0` or `1`);
> * After reaching `(N-1, N-1)`, returning to `(0, 0)` by moving left or up through valid path cells;
> * When passing through a path cell containing a cherry, you pick it up and the cell becomes an empty cell `(0)`;
> * If there is no valid path between `(0, 0)` and `(N-1, N-1)`, then no cherries can be collected.
>
> Example 1:
> ```
> Input: grid =
> [[0, 1, -1],
> [1, 0, -1],
> [1, 1,  1]]
> Output: 5
> Explanation: 
> The player started at (0, 0) and went down, down, right right to reach (2, 2).
> 4 cherries were picked up during this single trip, and the matrix > becomes [[0,1,-1],[0,0,-1],[0,0,0]].
> Then, the player went left, up, up, left to return home, picking up one more cherry.
> The total number of cherries picked up is 5, and this is the maximum possible.
>```

##### Solution

It's important to note that we can't maximize one leg of the trip then return to maximize the other. This is because we don't know which cherries have already been picked, and we'll end up over counting.

Instead, we just run two simultaneous paths from start to end. If we do this recursively, we end up with a runtime of $\small \mathcal O(4^{n^{2}})$. This is because for each of the $\small \mathcal O(n^{2})$ cells, we make $\small 4$ branches.

This is where we turn to dynamic programming - each of the states can be remembered by the coordinates: `r1`, `c1`, `r2`, `c2`. If we memoize each state by this key, we reduce time down to $\small \mathcal O(n^{4})$. However, we have to move each turn, and we are only allowed to move down or to the right. That means `r1 + c1 = r2 + c2`, and `r1 + c1` is strictly increasing. This allows us to drop `c2` from the function, because we can calculate it directly from `r1 + c1 - r2`. This brings our time complexity down to $\small \mathcal O(n^3)$.

```py
def cherryPickup(grid: "List[List[int]]") -> int:
    N = len(grid)
    memo = [[[None] * N for _1 in range(N)] for _2 in range(N)]
    
    def dp(r1, c1, r2):
        c2 = r1 + c1 - r2
        if r1 == N or c1 == N or r2 == N or c2 == N or grid[r1][c1] == -1 or grid[r2][c2] == -1:
            return float('-inf')
        elif r1 == c1 == N - 1:
            return grid[r1][c1]
        elif memo[r1][c1][r2] is not None:
            return memo[r1][c1][r2]
        else:
            pickup = grid[r1][c1] + (r1 != r2) * grid[r2][c2]
            pickup += max(dp(r1+1, c1, r2+1), dp(r1, c1+1, r2+1), dp(r1+1, c1, r2), dp(r1, c1+1, r2))
            memo[r1][c1][r2] = pickup
            return memo[r1][c1][r2]
    
    return max(0, dp(0, 0, 0))
```