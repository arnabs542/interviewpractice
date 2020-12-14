#### Search for a Sequence in a 2D Array

> Given a 2D board and a word, find if the word exists in the grid.
>
> The word can be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell **may** be used more than once.
>
> **Example:**
>
> ```
> board =
> [
>   ['A','B','C','E'],
>   ['S','F','C','S'],
>   ['A','D','E','E']
> ]
>
> Given word = "ABCCED", return true.
> Given word = "SEE", return true.
> Given word = "ABCB", return false.
> ```

##### DFS:

```py
def is_pattern_contained_in_grid(grid, S):

    def helper(i, j, idx):
        if idx == len(S):
            return True
        if not (0 <= i < len(grid) and 0 <= j < len(grid[0])) or grid[i][j] != S[idx]:
            return False
        return helper(i+1, j, idx+1) or helper(i, j+1, idx+1) or helper(i-1, j, idx+1) or helper(i, j-1, idx+1)

    for i in range(len(grid)):
        for j in range(len(grid[0])):
            if helper(i, j, 0):
                return True

    return False
```

A brute force solution would be to iterate through every single cell, attempting to start the word at that location. Let $\small l$ be the length of the word; it takes $\small \mathcal O(4^{l})$ time to find the word, assuming it exists, since we can go in four directions to search for each character. Overall runtime is $\small \mathcal O(n*m*4^{l})$, where $\small m,n$ are the dimensions of the grid.

##### Dynamic Programming:

```py
def is_pattern_contained_in_grid(grid, S):

    def is_pattern_suffix_contained_starting_at_ij(i, j, idx):
        if idx == len(S):
            # Nothing left to find
            return True

        if (0 <= i < len(grid) and 0 <= j < len(grid[i])) and grid[i][j] == S[idx] \
                and (i, j, idx) not in previous_attempts \
                and any(is_pattern_suffix_contained_starting_at_ij(i+a, j+b, idx+1) \
                        for a, b in ((-1, 0), (1, 0), (0, -1), (0, 1))):
            return True
        previous_attempts.add((i, j, idx))
        return False

    previous_attempts = set()

    return any(is_pattern_suffix_contained_starting_at_ij(i, j, 0) for i in range(len(grid)) \
               for j in range(len(grid[i])))
```

We can expect a lot of repeated work from a simple DFS approach. Thus, we use a cache to store intermediate work so we don't keep repeating ourselves. The key is `(i, j, idx)`. If we're not able to find the character at `S[idx]` at `i,j` or we're not able to finish `S[idx:]` starting at `i,j` then there's no point in doing it again when we encounter the same scenario next time. The time complexity is now reduced to $\small \mathcal O(mn|S|)$.

