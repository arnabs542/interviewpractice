#### Flip Equivalent Binary Trees

> For a binary tree T, we can define a flip operation as follows: choose any node, and swap the left and right child subtrees.
>
> A binary tree X is _flip equivalent_ to a binary tree Y if and only if we can make X equal to Y after some number of flip operations.
>
> Write a function that determines whether two binary trees are _flip equivalent_.  The trees are given by root nodes `root1` and `root2`.
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

```py
def flipEquiv(root1, root2):

    if root1 is root2:  # Both roots are null
        return True

    if not root1 or not root2 or root1.val != root2.val:
        return False

    return ((self.flipEquiv(root1.left, root2.left) and self.flipEquiv(root1.right, root2.right)) 
            or (self.flipEquiv(root1.left, root2.right) and self.flipEquiv(root1.right, root2.left)))
```

We could just do a standard preorder traversal on the two trees and check all possible combinations. This takes $$\small \mathcal O(min(n_{1},n_{2}))$$ time, where $$\small n_{1}, n_{2}$$ are the number of elements in each tree. Space is dictated by the recursion stack, and takes $$\small \mathcal O(min(h_{1},h_{2}))$$, where $$\small h_{1},h_{2}$$ are the height of the tree.

Canonical Form:

```py
def flipEquiv(root1, root2):

    if root1 is root2:  # Both roots are null
        return True

    if not root1 or not root2 or root1.val != root2.val:
        return False

    if root1.right:
        if not root1.left or root1.right.val < root1.left.val:
            root1.left, root1.right = root1.right, root1.left

    if root2.right:
        if not root2.left or root2.right.val < root2.left.val:
            root2.left, root2.right = root2.right, root2.left        

    return self.flipEquiv(root1.left, root2.left) and self.flipEquiv(root1.right, root2.right)
```



