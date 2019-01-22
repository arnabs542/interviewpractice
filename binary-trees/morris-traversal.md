#### Morris Inorder Traversal:

The inorder traversal of a binary tree can be done recursively or iteratively with a stack mimicking the recursion. However, both methods require $$\small O(h)$$ space, where $$\small h$$ is the height of the tree.

##### Algorithm:

```py
def inorderTraversal(root):
    """
    :type root: TreeNode
    :rtype: List[int]
    """
    def findPredecessor(root):
        it = root.left
        while it and it.right:
            if it.right == root:
                return it
            it = it.right
        return it

    cur = root
    res = []

    while cur:
        if not cur.left:
            res.append(cur.val)
            cur = cur.right
        else:
            pred = findPredecessor(cur)
            if not pred.right:
                pred.right = cur
                cur = cur.left
            else:
                pred.right = None
                res.append(cur.val)
                cur = cur.right

    return res
```



