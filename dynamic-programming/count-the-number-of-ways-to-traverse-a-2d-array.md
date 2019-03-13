#### Count the Number of Ways to Traverse a 2D Array

> Given a 2D array, you start at the top-left corner. Count the total number of ways to get to the bottom-right corner. All moves must either go right or down.

##### Dynamic Programming:

```py
def number_of_ways(n, m):

    matrix = [[0] * n for _ in range(m)]

    for i in range(n):
        matrix[0][i] = 1
    for i in range(m):
        matrix[i][0] = 1
    matrix[0][0] = 1

    for i in range(1, m):
        for j in range(1, n):
            matrix[i][j] += matrix[i-1][j] + matrix[i][j-1]
    return matrix[-1][-1]
```

Since we're only allowed to go right or down, then the number of ways to get to a certain cell is simply the number of ways to get to the cell above it and the cell to the left of it \(if they exist\). This optimal subproblem structure calls for dynamic programming. All cells in the first row and first column has only one way to get to it. Otherwise take the sum. 

Running time and space are both bounded by $$\small \mathcal O(n*m)$$.

