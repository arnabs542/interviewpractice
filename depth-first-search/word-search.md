#### Word Search

> Given a 2D board and a word, find if the word exists in the grid.
>
> The word can be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once.
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
def exist(board, word):
    
    def find(i, j, word, idx):
        if idx == len(word):
            return True            
        if not (0 <= i < len(board) and 0 <= j < len(board[0])) or board[i][j] != word[idx]:
            return False
        c, board[i][j] = board[i][j], '-'
        idx += 1
        if find(i+1, j, word, idx) or find(i, j+1, word, idx) or find(i-1, j, word, idx) or find(i, j-1, word, idx):
            return True
        board[i][j] = c
        return False
    
    for i in range(len(board)):
        for j in range(len(board[0])):
            if find(i, j, word, 0):
                return True
    
    return False
```

The algorithm itself is just a standard DFS. We iterate through the entire grid, and we try to find the word starting at each cell. The iteration takes $$\small \mathcal O(mn)$$ time. A single DFS search takes $$\small \mathcal O(4^{l})$$ time, where $$\small l$$ is the length of the word. This is because for each character in the string, we can go either up, down, left, or right. Actually we only have 3 choices for characters past the first one, since one of the directions is where we just came from. A stricter bound would be $$\small O(4*3^{l-1})$$. Overall runtime is $$\small \mathcal O(m*n*4^{l})$$.

