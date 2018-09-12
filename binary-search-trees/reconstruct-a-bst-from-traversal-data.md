#### Reconstruct a BST From Traversal Data

> Suppose you are given the keys of a BST in preorder traversal, and all keys are distinct. Can you reconstruct the BST from the sequence? Solve the same problem for inorder and postorder traversal sequences.

##### Code \(Unoptimized\):

```py
def rebuild_bst_from_preorder(preorder_sequence):
    if not preorder_sequence:
        return None
    transition_point = next((i for i, a in enumerate(preorder_sequence) if a > preorder_sequence[0]), len(preorder_sequence))
    root = BstNode(preorder_sequence[0])
    root.left = rebuild_bst_from_preorder(preorder_sequence[1:transition_point])
    root.right = rebuild_bst_from_preorder(preorder_sequence[transition_point:])
    return root
```

##### Explanation:

A preorder traversal of a tree goes `[root, preorder(root.left), preorder(root.right)]`. Since all values on the left of a BST must be smaller than the root, and all values on the right of a BST must be larger than the root, we simply need to find the transition point where the first element larger than the root value appears, split the array, and build the subtrees recursively. The problem with the above implementation is that we constantly iterate over the array, and this takes a lot of time. Suppose the tree is left-skewed. Each step of the recursive call would result in us doing only 1 unit of work, giving us the recurrence relationship $$\small T(n) = T(n-1) + \mathcal O(1)$$. This solves to a running time of $$\small O(n^{2})$$. In the event of a 

