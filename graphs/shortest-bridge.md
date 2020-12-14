#### Shortest Bridge

> In a given 2D binary array `A`, there are two islands.  \(An island is a 4-directionally connected group of `1`s not connected to any other 1s.\)
>
> Now, we may change `0`s to `1`s so as to connect the two islands together to form 1 island.
>
> Return the smallest number of `0`s that must be flipped.  \(It is guaranteed that the answer is at least 1.\)
>
> **Example 1:**
>
> ```
> Input: [[0,1],[1,0]]
> Output: 1
> ```
>
> **Example 2:**
>
> ```
> Input: [[0,1,0],[0,0,0],[0,0,1]]
> Output: 2
> ```
>
> **Example 3:**
>
> ```
> Input: [[1,1,1,1,1],[1,0,0,0,1],[1,0,1,0,1],[1,0,0,0,1],[1,1,1,1,1]]
> Output: 1
> ```

##### Code:

```py
def shortestBridge(A):

    def dfs(i, j):
        A[i][j] = 2
        queue.append((i, j))
        for x, y in ((i-1,j), (i+1,j), (i,j-1), (i,j+1)):
            if -1 < x < n and -1 < y < n and A[x][y] == 1:
                dfs(x, y)

    def first():
        for i in range(n):
            for j in range(n):
                if A[i][j]:
                    return i, j

    n, flips, queue = len(A), 0, collections.deque()
    dfs(*first())

    while queue:
        l = len(queue)
        for _ in range(l):
            i, j = queue.popleft()
            for x, y in ((i-1,j), (i+1,j), (i,j-1), (i,j+1)):
                if -1 < x < n and -1 < y < n:
                    if A[x][y] == 1:
                        return flips
                    elif A[x][y] == 0:
                        A[x][y] = 2
                        queue.append((x,y))
        flips += 1
```

##### Explanation:

This is a pretty straightforward DFS problem as well. First one find one island, and change all of its values from 1 to 2. Then we simply perform a BFS using all the nodes in that island. If the node borders an ocean tile, we flip the tile. If the node borders the other island, we return. Otherwise, we continue. Time complexity should be bounded by $$\small \mathcal O(A)$$, where $$\small A$$ is the size of the 2D array.

