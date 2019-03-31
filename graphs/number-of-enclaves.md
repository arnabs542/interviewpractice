#### Number of Enclaves

> Given a 2D array `A`, each cell is 0 \(representing sea\) or 1 \(representing land\)
>
> A move consists of walking from one land square 4-directionally to another land square, or off the boundary of the grid.
>
> Return the number of land squares in the grid for which we **cannot** walk off the boundary of the grid in any number of moves.
>
> **Example 1:**
>
> ```
> Input: [[0,0,0,0],[1,0,1,0],[0,1,1,0],[0,0,0,0]]
> Output: 3
> Explanation: 
> There are three 1s that are enclosed by 0s, and one 1 that isn't enclosed because its on the boundary.
> ```
>
> **Example 2:**
>
> ```
> Input: [[0,1,1,0],[0,0,1,0],[0,0,1,0],[0,0,0,0]]
> Output: 0
> Explanation: 
> All 1s are either on the boundary or can reach the boundary.
> ```

##### BFS \(Expand to boundary\):

```py
def numEnclaves(A: "List[List[int]]") -> int:
    can_reach, cant_reach = set(), set()

    def can_reach_boundary(i, j, can_reach, cant_reach):
        visited = set()

        bfs = collections.deque([(i,j)])
        while bfs:
            (i,j) = bfs.popleft()
            visited.add((i,j))
            if not (0 <= i < len(A) and 0 <= j < len(A[0])):
                can_reach |= visited
                return               
            for i,j in ((i+1,j), (i,j+1),(i-1,j),(i,j-1)):
                if (i,j) not in visited and (not (0 <= i < len(A) and 0 <= j < len(A[0])) or A[i][j] == 1):
                    visited.add((i,j))
                    bfs.append((i,j))
        cant_reach |= visited
        return

    # Grab all land squares
    land_squares = []
    for i in range(len(A)):
        for j in range(len(A[0])):
            if A[i][j] == 1:
                land_squares.append((i,j))

    # Test each square
    count = 0
    for i,j in land_squares:
        if (i,j) not in can_reach and (i,j) not in cant_reach:
            can_reach_boundary(i, j, can_reach, cant_reach)
            #print(i, j, cant_reach, can_reach)

    return len(cant_reach)
```

The main ideas of the above algorithm are:

1. Grab the locations of all land squares
2. For each land square, if it has not already been visited, try to reach the boundary
3. If the land square can reach the boundary, add all squares along its expansion path into the set `can_reach`. Otherwise, add all squares to `cant_reach`
4. Return the number of elements in `cant_reach`

The initial collection of all land squares takes $$\small \mathcal O(mn)$$ time. The bfs traversal takes $$\small \mathcal O(mn)$$ time for each square. But since we categorize all squares after visiting them once, overall runtime is $$\small \mathcal O(mn)$$.

##### DFS \(Expand inwards\):

```py
def dfs( A, row, col, i, j):
    if not (0 <= i < row and 0 <= j < col) or A[i][j] == 0:
        return
    A[i][j] = 0
    for i,j in ((i+1,j), (i,j+1), (i-1,j), (i,j-1)):
        dfs(A, row, col, i, j)

def numEnclaves(self, A: List[List[int]]) -> int:
    row, col = len(A), len(A[0])
    
    # Expand inwards from top and bottom row
    for r in [0, row-1]:
        for c in range(col):
            self.dfs(A, row, col, r, c)
    
    # Expand inwards from left and right cols
    for c in [0, col-1]:
        for r in range(row):
            dfs(A, row, col, r, c)
    
    # Find how many land squares are left
    count = 0
    for i in range(row):
        for j in range(col):
            count += A[i][j]
    
    return count
```

The idea of the above algorithm is essentially the same, except we go through all boundary squares and try to expand in. Each time we find a land square that is reachable from the boundaries, we "flood" it by turning it from a 1 to a 0. At the end, we see how many land squares are left. 

