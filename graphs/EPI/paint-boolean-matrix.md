#### Paint a Boolean Matrix

> A boolean matrix is a matrix whose cells are painted either white or black. Given a starting cell, flip its color and the color of all connected cells. A connected cell is defined as a cell that is reachable from the starting cell via a path that consists of all cells with the same color as the starting cell.

##### Solution

Since we're expanding out from a starting path, it makes sense to use BFS in this situation. We start the queue with the starting cell, then we add all neighboring cells whose colors are the same. As we add the neighbors, we flip their colors to prevent duplicates in our queue.

```py
def flip_color(x, y, image):
    queue = deque([(x,y)])
    color = image[x][y]
    directions = [(1,0), (0,1), (-1,0), (0,-1)]

    image[x][y] ^= 1

    while queue:
        x, y = queue.popleft()
        for dx, dy in directions:
            if 0 <= x + dx < len(image) and 0 <= y + dy < len(image[0]) and image[x+dx][y+dy] == color:
                queue.append((x+dx, y+dy))
                image[x+dx][y+dy] = color ^ 1
    return
```

The time complexity is the same as a standard BFS algorithm in an adjacency matrix; $\small \mathcal O(mn)$. Worst case we will have to visited every single cell. Space complexity is $\small \mathcal O(m + n)$, since there are at most $\small \mathcal O(m + n)$ vertices that are at the same distance from a given entry.