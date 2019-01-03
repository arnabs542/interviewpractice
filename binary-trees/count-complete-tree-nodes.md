#### Count Complete Tree Nodes

> Given a **complete** binary tree, count the number of nodes.
>
> **Note:**
>
> **Definition of a complete binary tree from **[**Wikipedia**](http://en.wikipedia.org/wiki/Binary_tree#Types_of_binary_trees)**:**  
>  In a complete binary tree every level, except possibly the last, is completely filled, and all nodes in the last level are as far left as possible. It can have between 1 and 2h nodes inclusive at the last level h.
>
> **Example:**
>
> ```
> Input:
>  
>     1
>    / \
>   2   3
>  / \  /
> 4  5 6
>
>
> Output: 6
> ```

##### Compare Heights:

```py
def countNodes(root):

    def left_height(root):
        count = 0
        while root:
            count += 1
            root = root.left
        return count

    def right_height(root):
        count = 0
        while root:
            count += 1
            root = root.right
        return count

    lh, rh = left_height(root), right_height(root)

    if lh == rh:
        return 2**lh - 1

    return self.countNodes(root.left) + self.countNodes(root.right) + 1
```

Of course we could perform a simple traversal of the tree and count all the nodes in $$\small \mathcal O(n)$$ time. However, this disregards the fact that we're told the tree is complete.

The above code is based on the realization that a complete tree has either a perfectleft subtree, a perfect right subtree, or both. If the left subtree is perfect, then we only need to know its height to compute the number of nodes within it, which requires only $$\small \mathcal O(\log{n})$$. The same applies for the right subtree.

The running time of the algorithm is bounded by $$\small \mathcal O((\log{n})^{2})$$. We halve the tree each iteration, and each iteration requires $$\small \mathcal O(\log {n})$$ time to calculate the height.

Another way to calculate the running time is to expand the recursion relationship:
$$
\small T(h) = O(h) + 2T(n-1)  \\ \small = \mathcal O(h) + \mathcal O(h-1) + \mathcal O(h-1) + 2T(h-2) \\ \small = \mathcal O(h) + 2 \mathcal O(h-1) + 2 \mathcal O(h-2) + ... \mathcal O(2) + \mathcal O(1)
$$
Since $$\small \mathcal O(h) = \mathcal O(\log{n})$$, we can rewrite the equation above as $$ \small \mathcal O\(h\) + 2 \mathcal O\(h-1\) + 2 \mathcal O\(h-2\) + ... \mathcal O\(2\) + \mathcal O\(1\)$$

