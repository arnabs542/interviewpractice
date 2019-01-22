#### Morris Inorder Traversal:

The inorder traversal of a binary tree can be done recursively or iteratively with a stack mimicking the recursion. However, both methods require $$\small O(h)$$ space, where $$\small h$$ is the height of the tree. The Morris traversal is a traversal that uses constant space.

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

The Morris tree essentially turns each binary tree into a threaded tree - each node has a pointer to its inorder successor. Take the tree below as an example:

![](/assets/morris_traversal_inorder.png)

We begin at node H, which is the root. The inorder traversal of the node would be $$\small [A, B, C, D, E, F, G, H, ...]$$. Thus, the inorder predecessor of H is G. So we link the right child of G to H, so after traversing G, we move on to H. 

The overall runtime complexity of the algorithm is still $$\small O(n)$$, $$\small n$$ being the number of nodes in the tree. This is because each node is only visited at most 3 times. Take for example the node G. We first visit it when we look for the predecessor of H. The second time we visit it is when we visit it to print the value. The third time we visit it is when we move back to H, and we see that its predecessor points to itself, and we remove the link we added earlier.

