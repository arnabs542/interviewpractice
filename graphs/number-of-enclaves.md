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

##### BFS:

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



