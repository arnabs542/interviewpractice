#### Generate All Non-attacking Placement of n-Queens

> The n-queens puzzle is the problem of placing n queens on an n×n chessboard such that no two queens attack each other.
> 
> ![](../../assets/n-queens.png)
> 
> Given an integer n, return all distinct solutions to the n-queens puzzle.

##### Solution

The real difficulty of this problem is coming up with a way to efficiently check whether a position attacks any previously placed queens.

```py
def n_queens(n):
    def solve_n_queens(row):
        if row == n:
            # All queens place
            result.append(list(col_placement))
            return
        for col in range(n):
            if all (abs(c - col) not in (0, row - i) for i,c in enumerate(col_placement)):
                col_placement[row] = col
                solve_n_queens(row + 1)
    
    result, col_placement = [], [0] * n
    solve_n_queens(0)
    return result
```

Since we know we can only have exactly 1 queen on each row, we can just iterate through the rows and try to place a queen there. In order to check for conflicts, we need to check all previous rows for queens in either diagonal or directly above. The line
```py
if all (abs(c - col) not in (0, row - i) for i,c in enumerate(col_placement))
```
does the check in an extremely clever way. In the row above, the diagonal, if it exists, must be in either `(col-1, col+1)`. In the row 2 above, the diagonal would be in `(col-2, col+2)`. 

In otherwords, the distance between the current row and the checked row must be the same distance between the current column and checked column.