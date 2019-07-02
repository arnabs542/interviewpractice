#### Surrounded Regions

> Given a 2D board containing `X` and `O` (the letter O), capture all regions surrounded by `X`.
>
> A region is captured by flipping all `O`s into `X`s in that surrounded region.
>
> Example:
> ```
> X X X X
> X O O X
> X X O X
> X O X X
>```
> After running your function, the board should be:
>```
> X X X X
> X X X X
> X X X X
> X O X X
>```
> Explanation:
>
> Surrounded regions shouldn’t be on the border, which means that any `O` on the border of the board are not flipped to `X`. Any `O` that is not on the border and it is not connected to an `O` on the border will be flipped to `X`. Two cells are connected if they are adjacent cells connected horizontally or vertically.

##### Solution

This is a standard BFS/DFS problem, but we can speed up our average running time by doing the inverse. Instead of starting from every untraversed `O` cell and seeing if we can reach the border, we should start from every border `O` cell and expand inwards. This is because in the cases where we have large surrounded regions, we will end up spending a lot of time doing useless work in the first scenario, whereas if we move inwards, we will skip those spots since they aren't connected to the border.

```py
def solve(board: "List[List[str]]") -> None:

    if not board or not board[0]:
        return
    
    bfs = collections.deque()
    n, m = len(board), len(board[0])
    
    for i in range(n):
        for j in (0, m-1):
            if board[i][j] == 'O':
                bfs.append((i,j))
    
    for j in range(m):
        for i in (0, n-1):
            if board[i][j] == 'O':
                bfs.append((i,j))
    
    while bfs:
        x,y = bfs.popleft()
        board[x][y] = 'S'
        for dx, dy in ((0,1), (1,0), (-1,0), (0,-1)):
            if 0 <= dx + x < n and 0 <= dy + y < m and board[dx + x][dy + y] == 'O':
                board[dx + x][dy + y] = 'S'
                bfs.append((dx + x, dy + y))
                
    for i in range(n):
        for j in range(m):
            board[i][j] = 'O' if board[i][j] == 'S' else 'X'
```

Worst case running time is still $\small \mathcal O(mn)$, since we might have to visit every single cell.

We could also use a DSU to solve this problem - assign a sentinal node for the border and connect cells accordingly.