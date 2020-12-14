#### Is Graph Bipartite

> Given an undirected `graph`, return `true` if and only if it is bipartite.
>
> Recall that a graph is _bipartite_ if we can split it's set of nodes into two independent subsets A and B such that every edge in the graph has one node in A and another node in B.
>
> The graph is given in the following form: `graph[i]` is a list of indexes `j` for which the edge between nodes `i` and `j` exists.  Each node is an integer between `0` and `graph.length - 1`.  There are no self edges or parallel edges: `graph[i]` does not contain `i`, and it doesn't contain any element twice.
>
> ```
> Example 1:
> Input:
>  [[1,3], [0,2], [1,3], [0,2]]
>
> Output:
>  true
>
> Explanation:
>  
> The graph looks like this:
> 0----1
> |    |
> |    |
> 3----2
> We can divide the vertices into two groups: {0, 2} and {1, 3}.
> ```
>
> ```
> Example 2:
> Input:
>  [[1,2,3], [0,2], [0,1,3], [0,2]]
>
> Output:
>  false
>
> Explanation:
>  
> The graph looks like this:
> 0----1
> | \  |
> |  \ |
> 3----2
> We cannot find a way to divide the set of nodes into two independent subsets.
> ```

##### Code:

```py
def isBipartite(self, graph):

    # build graph
    g = [-1] * len(graph)

    # dfs
    def color(node, c):
        if g[node] != -1:
            return g[node] == c
        g[node] = c
        for nei in graph[node]:
            if not color(nei, c ^ 1):
                return False
        return True

    for i in range(len(graph)):
        if g[i] == -1:
            if not color(i, 0):
                return False
    return True
```

##### Explanation:

The is a very straightforward dfs problem. We simply attempt to color every single node, swapping color for its neighbors. We when find we come across an already colored node, we check to see if that color we previously assigned it matches what we were going to assign it. If yes, return True. Else return False. Running time is bounded by $$\small \mathcal O(V + E)$$.

