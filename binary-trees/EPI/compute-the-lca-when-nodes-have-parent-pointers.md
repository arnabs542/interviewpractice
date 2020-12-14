#### Compute the LCA when Nodes have Parent Pointers

> Given two nodes in a binary tree, design an algorithm that computes their LCA. Assume that each node has a parent pointer.

Follow up to previous question. This can be easily solved using a hash table to store the ancestors of one path, and then traverse the other path and return when we come across an ancestor or reach the root.

##### Code:

```py
def lca(node0, node1):
    def get_depth(node):
        depth = 0
        while node:
            depth += 1
            node = node.parent
        return depth

    depth0, depth1 = map(get_depth, (node0, node1))
    # Make node0 as the deeper node in order to simply the code
    if depth1 > depth0:
        node0, node1 = node1, node0

    # Ascend from deeper node
    depth_diff = abs(depth0 - depth1)
    while depth_diff:
        node0 = node0.parent
        depth_diff -= 1

    # Now ascend both nodes in tandem
    while node0 is not node1:
        node0, node1 = node0.parent, node1.parent
    return node0
```

##### Explanation:

Since the two nodes are given from the same tree, we know that at the very worst, the root of the tree will be the ancestor. If the nodes are at the same depth, we can move up the tree in tandem. If they are not at the same depth, we simply need to move the deeper node up to the same depth and then we can proceed.

Time complexity is bounded by $\small \mathcal O(h)$. We don't use any extra space.

