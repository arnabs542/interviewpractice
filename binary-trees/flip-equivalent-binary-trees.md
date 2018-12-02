####  Flip Equivalent Binary Trees

> For a binary tree T, we can define a flip operation as follows: choose any node, and swap the left and right child subtrees.
>
> A binary tree X is _flip equivalent_ to a binary tree Y if and only if we can make X equal to Y after some number of flip operations.
>
> Write a function that determines whether two binary trees are _flip equivalent_.  The trees are given by root nodes `root1` and `root2`.
>
> ![](/assets/flip_equiv.png)
>
> **Example 1:**
>
> ```
> Input: root1 = [1,2,3,4,5,6,null,null,null,7,8], root2 = [1,3,2,null,6,4,5,null,null,null,null,8,7]
> Output: true
> Explanation: We flipped at nodes with values 1, 3, and 5.
> ```

##### DFS:

```
def flipEquiv(root1, root2):

    if root1 is root2:  # Both roots are null
        return True

    if not root1 or not root2 or root1.val != root2.val:
        return False

    return ((self.flipEquiv(root1.left, root2.left) and self.flipEquiv(root1.right, root2.right)) 
            or (self.flipEquiv(root1.left, root2.right) and self.flipEquiv(root1.right, root2.left)))     
```





