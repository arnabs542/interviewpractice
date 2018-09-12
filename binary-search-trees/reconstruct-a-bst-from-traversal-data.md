#### Reconstruct a BST From Traversal Data

> Suppose you are given the keys of a BST in preorder traversal, and all keys are distinct. Can you reconstruct the BST from the sequence? Solve the same problem for inorder and postorder traversal sequences.

##### Code \(Unoptimized\):

```
def rebuild_bst_from_preorder(preorder_sequence):
    if not preorder_sequence:
        return None
    root = BstNode(preorder_sequence[0])
    root.left = rebuild_bst_from_preorder([x for x in preorder_sequence[1:] if x < root.data])
    root.right = rebuild_bst_from_preorder([x for x in preorder_sequence[1:] if x > root.data])
    return root
```



