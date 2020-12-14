#### Clone a Graph

> Consider a vertex type for a directed graph in which there are two fields: an integer label and a list of references to other vertices. Design an algorithm that takes a reference to a vertex $\small u$, and creates a copy of the graph of the vertices reachable from $\small u$. Return the copy of $\small u$.

##### Solution

We can do this either with BFS or DFS. Below is the BFS solution.

```py
class GraphVertex:
    def __init__(self, label):
        self.label = label
        self.edges = []


def clone_graph(graph):
    if not graph:
        return None
    
    q = collections.deque([graph])
    vertex_map = {graph: GraphVertex(graph.label)}

    while q:
        v = q.popleft()
        for e in v.edges:
            # Try to copy e
            if e not in vertex_map:
                vertex_map[e] = GraphVertex(e.label)
                q.append(e)
            # Copy edge v->e
            vertex_map[v].edges.append(e)
    
    return vertex_map[graph]
```
Running time is $\small \mathcal O(|V| + |E|)$, which is standard for BFS. Space complexity is the same, since we copy very single node and edge.

Below is the dfs solution:

```py
def clone_graph(graph):
    if not graph:
        return None

    mapping = {}

    def dfs(graph, mapping):
        if graph in mapping:
            return mapping[graph]
        
        mapping[graph] = GraphVertex(graph.label)
        for e in graph.edges:
            mapping[graph].edges.append(dfs(e, mapping))
        return mapping[graph]
    
    return dfs(graph, mapping)
```

The entire idea is to traverse the graph, maintaining a mapping between the original node and its clone. 