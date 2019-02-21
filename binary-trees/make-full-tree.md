#### Make Full Tree

> Recall that a full binary tree is one in which each node is either a leaf node, or has two children. Given a binary tree, convert it to a full one by removing nodes with only one child.
>
> For example, given the following tree:
>
> ```
>          0
>       /     \
>     1         2
>   /            \
> 3                 4
>   \             /   \
>     5          6     7
> ```
>
> You should convert it to:
>
> ```
>      0
>   /     \
> 5         4
>         /   \
>        6     7
> ```

##### Postorder Pruning:

```py
def makeFull(node):
    if not node or (node.left is node.right is None):
        return node
    left, right = makeFull(node.left), makeFull(node.right)
    if left is not None and right is not None:
        node.left, node.right = left, right
        return node
    elif not right:
        return left
    elif not left:
        return right
```

We prune the tree recursively, and return either `None` or full nodes. The variables `left, right` represent the pruned left and right subtrees. If both subtrees are not null objects, then there are full trees on both side of the current node. In that case, set `left,right` as the left and right subtrees of the current node and return it. Otherwise, return whatever isn't null, or return either if both are null.

Running time is $$\small \mathcal O(n)$$. Space is $$\small \mathcal O(h)$$.

