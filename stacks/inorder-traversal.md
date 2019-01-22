

Stack:

```
def inorderTraversal(self, root):
    """
    :type root: TreeNode
    :rtype: List[int]
    """
    it, stack, res = root, [], []
    
    while stack or it:
        if it:
            stack.append(it)
            it = it.left
        else:
            it = stack.pop()
            res.append(it.val)
            it = it.right
    
    return res
```



