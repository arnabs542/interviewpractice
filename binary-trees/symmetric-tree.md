#### Symmetric Tree

> Given a binary tree, check whether it is a mirror of itself \(ie, symmetric around its center\).
>
> For example, this binary tree `[1,2,2,3,4,4,3]` is symmetric:
>
> ```
>     1
>    / \
>   2   2
>  / \ / \
> 3  4 4  3
> ```
>
> But the following `[1,2,2,null,3,null,3]` is not:
>
> ```
>     1
>    / \
>   2   2
>    \   \
>    3    3
> ```

##### Recursive Solution:

```py
def isSymmetric(root: 'TreeNode') -> 'bool':
    if not root:
        return True

    def dfs(node1, node2):
        if node1 is node2 is None:
            return True
        if not node1 or not node2:
            return False
        if node1.val != node2.val:
            return False
        return dfs(node1.left, node2.right) and dfs(node1.right, node2.left)

    return dfs(root.left, root.right)
```

Recursive dfs solution. Compare the paired children as we traverse the tree. Run time is $\small \mathcal O(n)$, space usage is $\small \mathcal O(h)$.

##### Iterative Solution:

```py
def isSymmetric(root: 'TreeNode') -> 'bool':
    if not root:
        return True
    
    stack = [(root.left, root.right)]
    while stack:
        print(stack)
        node1, node2 = stack.pop()
        if node1 is node2 is None:
            continue
        elif node1 and node2 and node1.val == node2.val:
            stack.append((node1.left, node2.right))
            stack.append((node1.right, node2.left))
        else:
            return False
    return True
```

Iterative solution using stack to mimic recursive call stack. Had to change the if/else conditions a bit to make it work better. 

