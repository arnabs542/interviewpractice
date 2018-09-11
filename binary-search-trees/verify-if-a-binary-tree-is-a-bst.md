#### Verify if a Binary Tree is a BST

> Write a program that takes as input a binary tree and checks if the tree satisfies the BST property.

##### Code:

```py
def is_binary_tree_bst(tree, low_range=float('-inf'), high_range=float('inf')):
    if not tree:
        return True
    if tree.data < low_range or tree.data > high_range:
        return False
    return is_binary_tree_bst(tree.left, low_range, tree.data) and is_binary_tree_bst(tree.right, tree.data, high_range)
```

##### Explanation:

The BST property is a global property, so we can simply check the tree recursively against the current bounds relative to the node.

Time complexity is $$\small \mathcal O(n)$$, where $$\small n$$ is the number of nodes in the tree. 

