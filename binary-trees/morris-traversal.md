#### Morris Inorder Traversal:



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



