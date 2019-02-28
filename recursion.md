#### Implement a Sudoku Solver

> Implement a sudoku solver. Unfilled spaces are initialized populated with 0.

##### Brute Force:

```py
def solve_sudoku(partial_assignment):
    # Get coordinates of all empty squares
    spaces = []
    for i in range(len(partial_assignment)):
        for j in range(len(partial_assignment[0])):
            if partial_assignment[i][j] == 0:
                spaces.append((i, j))

    checker = Checker()

    def helper(partial_assignment, left):
        if len(left) == 0:  # Finished solving partial_assignment
            return True
        x, y = left[0]
        # For each empty square, try to put values down until we can solve the entire board
        for v in range(1, 10):
            partial_assignment[x][y] = v
            if checker.isValidSudoku(partial_assignment):
                if helper(partial_assignment, left[1:]):
                    return True
        partial_assignment[x][y] = 0
        return False

    helper(partial_assignment, spaces)
    return True
```



