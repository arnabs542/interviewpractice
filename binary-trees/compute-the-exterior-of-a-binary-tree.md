##### Compute the Exterior of a Binary Tree

> The exterior of a binary tree is the following sequence of nodes: the nodes from the root to the leftmost leaf, followed by the leaves in left-to-right order, followed by the nodes from the rightmost leaf to the root. For example, the exterior of the binary tree below is &lt;2, 7, 2, 5, 11, 4, 9, 5&gt;.
>
> ![](/assets/binary_tree.png)

The main trick of this problem is to use the structure of the tree itself to help us avoid duplicates. If we directly process on the root, we run into situations where the leftmost leaf node appears in both the left side and the leaf traversals, similarly for the rightmost leaf node. If the tree is completely tilted like a linked list, we need to figure out how to prevent nodes from being replicated.

##### Code:

```py
def exterior_binary_tree(tree):
    def is_leaf(node):
        return node and not node.left and not node.right

    def left_boundary_and_leaves(subtree, is_boundary):
        if not subtree:
            return []
        return (([subtree] if is_boundary or is_leaf(subtree) else []) +
                left_boundary_and_leaves(subtree.left, is_boundary) +
                left_boundary_and_leaves(subtree.right, is_boundary and not subtree.left))

    def right_boundary_and_leaves(subtree, is_boundary):
        if not subtree:
            return []
        return (right_boundary_and_leaves(subtree.left, is_boundary and not subtree.right) +
                right_boundary_and_leaves(subtree.right, is_boundary) +
                ([subtree] if is_boundary or is_leaf(subtree) else []))

    return [] if not tree else ([tree] + left_boundary_and_leaves(tree.left, True) + 
                                right_boundary_and_leaves(tree.right, True))
```

##### Explanation:

To avoid having to figure out how to deal with duplicates ourselves, we simply use the tree structure. By performing the traversals on the root.left and root.right nodes, we ensure that nodes appear only once in the result. 

On the left side, since we want to go from the root to the leftmost leaf, then across to the rightmost leaf, we use a preorder traversal. 

On the right side, since we want to go from the leftmost leaf to the rightmost leaf, then up towards the root, we use a postorder traversal.

