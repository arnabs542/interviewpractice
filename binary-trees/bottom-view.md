#### Bottom View

> The horizontal distance of a binary tree node describes how far left or right the node will be when the tree is printed out.
>
> More rigorously, we can define it as follows:
>
> *     The horizontal distance of the root is 0.
> *     The horizontal distance of a left child is hd\(parent\) - 1.
> *     The horizontal distance of a right child is hd\(parent\) + 1.
>
> For example, for the following tree, hd\(1\) = -2, and hd\(6\) = 0.
>
> ```
>        5
>      /   \
>     3     7
>    / \   / \
>   1   4 6   9
>  /     /
> 0     8
> ```
>
> The bottom view of a tree, then, consists of the lowest node at each horizontal distance. If there are two nodes at the same depth and horizontal distance, either is acceptable.
>
> For this tree, for example, the bottom view could be \[0, 1, 3, 6, 8, 9\].
>
> Given the root to a binary tree, return its bottom view.

##### DFS:

```py
def bottomView(root):

    memo = {}
    
    def helper(root, depth, h_dist):
        if not root:
            return
        if h_dist not in memo:
            memo[h_dist] = (depth, root.val)
        elif memo[h_dist][0] < depth:
            memo[h_dist] = (depth, root.val)
        helper(root.left, depth+1, h_dist-1)
        helper(root.right, depth+1, h_dist+1)
    
    helper(root, 0, 0)

    return [memo[k][1] for k in memo]
```



