#### Search a Maze

> Consider a black and white digitized image of a maze - white pixels represent open areas and black spaces are walls. There are two special white pixels: one is designated the entrace and the other is the exit. The goal is to find a way from the entrance to the exit, if one exists.

##### Solution

Conceptually there's not much to this problem - simply apply a DFS search and see if a path exists. We could also use a BFS, but the problem doesn't ask for shortest path, so we let the compiler implictly handle the stack.

```py
WHITE, BLACK = range(2)

Coordinate = collections.namedtuple('Coordinate', ('x', 'y'))


def search_maze(maze, s, e):

    path = []

    def dfs(cur, target, path, maze, directions):
        if not (0 <= cur.x < len(maze) and 0 <= cur.y < len(maze[cur.x]) and maze[cur.x][cur.y] == WHITE):
            return False
        path.append(cur)
        maze[cur.x][cur.y] = BLACK
        if cur == e:
            return True
        for dx, dy in directions:
            if dfs(Coordinate(cur.x + dx, cur.y + dy), target, path, maze, directions):
                return True
        path.pop()
        return False
    
    if dfs(s, e, path, maze, directions = ((1,0), (-1, 0), (0, 1), (0, -1))):
        return path
    return []
```

Running time is $\small \mathcal O(|V| + |E|)$, same as DFS.