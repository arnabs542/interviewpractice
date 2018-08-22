#### Compute the LCA, Optimizing for Close Ancestors

> We previously did a problem where we had to compute the LCA when nodes have parent pointers in time proportional to the height of the tree. However, the solution required that we compute the path from each node to the root regardless of close their LCA was. 
>
> Design an algorithm for computing the LCA of two nodes in a binary tree. The algorithm's time complexity should depend only on the distance from the nodes to the LCA.

##### Code:

```py
def lca(node0, node1):
    iter0, iter1 = node0, node1
    nodes_on_path_to_root = set()
    while iter0 or iter1:
        # Ascend tree in tandem for these two nodes
        if iter0:
            if iter0 in nodes_on_path_to_root:
                return iter0
            nodes_on_path_to_root.add(iter0)
            iter0 = iter0.parent
        if iter1:
            if iter1 in nodes_on_path_to_root:
                return iter1
            nodes_on_path_to_root.add(iter1)
            iter1 = iter1.parent
    raise ValueError("node0 and node1 not in same tree")
```

##### Explanation: 

Advance the two nodes in tandem, recording everything from both paths to the root. If the two nodes are in the same tree, then eventually we'll come across a node that we've seen before already. That is the LCA. 

