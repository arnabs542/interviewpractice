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

The main source of optimization we can perform is on the checker. The above solution uses a checker that constantly goes over every single row, column, and subgrid after each added value, regardless of whether or not the value affects it. Since we are adding a value to an array that currently satisfies the constraints, we should only check the row, column, and subgrid that contains the added entry.

##### Optimized Backtracking:

```py
def solve_sudoku(partial_assignment):
    def solve_partial_sudoku(i, j):
        if i == len(partial_assignment):
            i = 0  # Starts a row.
            j += 1
            if j == len(partial_assignment[i]):
                return True  # Entire matrix has been filled without conflict.

        # Skips nonempty entries.
        if partial_assignment[i][j] != EMPTY_ENTRY:
            return solve_partial_sudoku(i + 1, j)

        def valid_to_add(i, j, val):
            # Check row constraints.
            if any(val == partial_assignment[k][j]
                   for k in range(len(partial_assignment))):
                return False

            # Check column constraints.
            if val in partial_assignment[i]:
                return False

            # Check region constraints.
            region_size = int(math.sqrt(len(partial_assignment)))
            I = i // region_size
            J = j // region_size
            return not any(
                val == partial_assignment[region_size * I + a][region_size * J + b]
                for a, b in itertools.product(range(region_size), repeat=2))

        for val in range(1, len(partial_assignment) + 1):
            # It's substantially quicker to check if entry val with any of the
            # constraints if we add it at (i,j) adding it, rather than adding it and
            # then checking all constraints. The reason is that we know we are
            # starting with a valid configuration, and the only entry which can
            # cause a problem is entry val at (i,j).
            if valid_to_add(i, j, val):
                partial_assignment[i][j] = val
                if solve_partial_sudoku(i + 1, j):
                    return True
        partial_assignment[i][j] = EMPTY_ENTRY  # Undo assignment.
        return False

    EMPTY_ENTRY = 0
    return solve_partial_sudoku(0, 0)
```



