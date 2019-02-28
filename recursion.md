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

The above solution makes use of a checker class which checks the validity of the board so far. 

We begin by grabbing the \(x,y\) coordinates of all unfilled spaces. For each of the spaces, we iterate through the number s 1-9, attempting to put each of them down to fill the cell. If the number we choose is valid, we continue onto the next cell. If there are no spaces left, we return True, signaling that we finished filling out the board. Otherwise, we try with the number number.

Running time should be $$\small \mathcal O(m*n + s^{9})$$, where $$\small m,n$$ represent the dimensions of the board, and $$\small s$$ the number of spaces the board initially has. We have to iterate over all values first to grab the unfilled squares, giving us a coordinate list of size $$\small s$$. For each of these cells, there's 9 values we can try, giving us the $$\small s^{9}$$ component. Space should be bounded by $$\small s$$, since the recursion stack will reach at most depth $$\small s$$.

