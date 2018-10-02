#### Compute the Lowest Common Ancestor in a Binary Tree

> Design an algorithm for computing the LCA of two nodes in a binary tree in which nodes do not have a parent field.

##### Code:

```py
def lca(tree, node0, node1):
    Status = collections.namedtuple('Status', ('num_target_nodes', 'ancestor'))

    # Returns an object consisting of an int and a node. The int field is 0, 1,
    # or 2 depending on how many of {node0, node1} are present in tree.
    # If both are present in tree, when ancestor is assigned to a non-null value, it is the lca

    def lca_helper(tree, node0, node1):
        if not tree:
            return Status(0, None)

        left_result = lca_helper(tree.left, node0, node1)
        if left_result.num_target_nodes == 2:
            # Found both nodes in left subtree
            return left_result
        right_result = lca_helper(tree.right, node0, node1)
        if right_result.num_target_nodes == 2:
            # Found both nodes in right subtree
            return right_result
        num_target_nodes = (left_result.num_target_nodes + right_result.num_target_nodes + (node0, node1).count(tree))
        return Status(num_target_nodes, tree)

    return lca_helper(tree, node0, node1).ancestor
```

##### Explanation:



