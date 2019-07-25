#### Flatten Binary Tree to Linked List

> Given a binary tree, flatten it to a linked list in-place.
>
> For example, given the following tree:
>
> ```
>     1
>    / \
>   2   5
>  / \   \
> 3   4   6
> ```
>
> The flattened tree should look like:
>
> ```
> 1
>  \
>   2
>    \
>     3
>      \
>       4
>        \
>         5
>          \
>           6
> ```

##### Code:

```py
class Solution:
    def __init__(self):
        self.prev = None

    def flatten(self, root):
        if not root:
            return
        self.flatten(root.right)
        self.flatten(root.left)
        root.right = self.prev
        self.prev = root
        root.left = None
```

##### Explanation:

Notice the order in which the flattened tree nodes appears - it's same order as a preorder traversal of the original tree. Therefore, if we traverse the tree in a reverse preorder, then we can build our list in reverse. We initialize an extra variable to keep track of which node we just came from, then adjust the tree before moving up. 

Running time: $\small \mathcal O(n)$ Space: $\small \mathcal O(1)$

