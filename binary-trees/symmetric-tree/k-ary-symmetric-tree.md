#### k-ary Symmetric Tree

> A tree is symmetric if its data and shape remain unchanged when it is reflected about the root node. The following tree is an example:
>
> ```
>         4
>       / | \
>     3   5   3
>   /           \
> 9              9
>
> ```
>
> Given a `k`-ary tree, determine whether it is symmetric.

##### Recursive:

```py
def isSymmetric(root):
    if not root:
        return True
    
    def dfs(node1, node2):
        if node1 is node2 is None:
            return True
        if node1 is None or node2 is None or node1.val != node2.val:
            return False
        i, j = 0, len(node2.children) - 1
        while i < len(node1.children) and j > -1:
            if not dfs(node1.children[i], node2.children[j]):
                return False
            i += 1
            j -= 1
        return True
    
    return dfs(root, root)
```

Same idea as the recursive solution for the binary tree, except we need to loop through the children using two pointer. Running time is $$\small \mathcal O(n)$$, height is $$\small \mathcal O(h)$$.

