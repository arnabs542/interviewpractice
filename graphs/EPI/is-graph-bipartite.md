#### Is Graph Bipartite

> Given an undirected graph, return true if and only if it is bipartite.
>
> Recall that a graph is bipartite if we can split it's set of nodes into two independent subsets `A` and `B` such that every edge in the graph has one node in `A` and another node in `B`.
>
> The graph is given in the following form: `graph[i]` is a list of indexes `j` for which the edge between nodes `i` and `j` exists.  Each node is an integer between `0` and `graph.length - 1`.  There are no self edges or parallel edges: `graph[i]` does not contain `i`, and it doesn't contain any element twice.
>
> Example 1:
> ```
> Input: [[1,3], [0,2], [1,3], [0,2]]
> Output: true
> Explanation: 
> The graph looks like this:
> 0----1
> |    |
> |    |
> 3----2
> ```
> We can divide the vertices into two groups: `{0, 2}` and `{1, 3}`.
>
> Example 2:
> ```
> Input: [[1,2,3], [0,2], [0,1,3], [0,2]]
> Output: false
> Explanation: 
> The graph looks like this:
> 0----1
> | \  |
> |  \ |
> 3----2
> ```
> We cannot find a way to divide the set of nodes into two independent subsets.

##### Solution

The brute force solution would try every different partition and see if it is possible. Not only is this expensive in time ($\small \mathcal O(2^{n})$), this is also nontrivial to code. 

Instead, it's much easier to simply try to assign colors. A bipartite graph can be divided into two sets of vertices which are disjoint and exhuastive such that there are no edges between the two sets. We should be able to assign one color to each of the two sets. This is possible because there are no edges between vertices in the same set in a bipartite graph. Therefore, let us pick a node that hasn't been visited, assign it a color, and then try to assign all its neighbors opposite colors. If we come across a neighbor that already has a color assigned, we check if that color is the same as the one we were going to assign. If no, then we have run into a contradition, and can safely return `False`.

Below is the DFS solution:

```py
def isBipartite(graph: "List[List[int]]") -> bool:
    colors = [-1] * len(graph)

    def dfs(graph, vertex, color, colors):
        if colors[vertex] != -1:
            return colors[vertex] == color
        colors[vertex] = color
        for neighbor in graph[vertex]:
            if not dfs(graph, neighbor, color ^ 1, colors):
                return False
        return True
    
    for i in range(len(colors)):
        if colors[i] == -1:
            if not dfs(graph, i, 0, colors):
                return False
    
    return True
```

Running time is $\small \mathcal O(|V| + |E|)$. Below is the BFS solution:

```py
def isBipartite(graph: "List[List[int]]") -> bool:
    colors = [-1] * len(graph)

    def bfs(graph, node, colors):
        q = collections.deque([(node, 0)])
        while q:
            cur_node, cur_color = q.popleft()
            colors[cur_node] = cur_color
            for neighbor in graph[cur_node]:
                if colors[neighbor] != -1:
                    if colors[neighbor] != cur_color ^ 1:
                        return False
                else:
                    colors[neighbor] = cur_color ^ 1
                    q.append((neighbor, cur_color ^ 1))
        return True

    for i in range(len(colors)):
        if colors[i] == -1:
            if not bfs(graph, i, colors):
                return False

    return True
```