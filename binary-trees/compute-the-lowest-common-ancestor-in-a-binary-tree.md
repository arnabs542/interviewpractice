#### Compute the Lowest Common Ancestor in a Binary Tree

> Any two nodes in a binary tree have a common ancestor, namely the root. The lowest common ancestor \(LCA\) of any two nodes in a binary tree is the node furthest from the root that is an ancestor of both nodes. 
>
> Computing the LCA has many important applications. For example, it is an essential calculation when rendering web pages, specifically when computing the Cascading Style Sheet \(CSS\) that is applicable to a particular Document Object Mode \(DOM\) element. 
>
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

The brute force solution would be to write a helper function to check if a node exists as a children to a root. We then check to see if both nodes are on the left - if so, recursively solve with the left node as the root. We perform a similar check for if both nodes are on the right. Only when one node is in the left and one node in the right do we return the root. However, in the case of a skewed tree \(linked-list structure\), our time complexity is $$\small \mathcal O(n^{2})$$, since we keep iterating through the list over and over, decreasing by 1 node each iteration. 

Instead, we can solve this problem in $$\small \mathcal O(n)$$ time and $$\small \mathcal O(h)$$ space by putting a bit more effort in how we return our search result. Instead of simply returning a True/False indicating if a node exists, we return a Status that tells us exactly how many of the nodes were found in the subtrees and the potential ancestor. This way, if we get a Status from the left subtree with `num_target_nodes == 2`, then we can immediately return that search result. Similar logic applies for the right subtree status. Finally, we return the current root, and check if it equals any of our targets.



