#### Binary Tree Maximum Path Sum

> Given a **non-empty** binary tree, find the maximum path sum.
>
> For this problem, a path is defined as any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The path must contain **at least one node** and does not need to go through the root.
>
> **Example 1:**
>
> ```
> Input: [1,2,3]
>
>        1
>       / \
>      2   3
>
> Output: 6
> ```
>
> **Example 2:**
>
> ```
> Input: [-10,9,20,null,null,15,7]
>
>    -10
>    / \
>   9  20
>     /  \
>    15   7
>
> Output: 42
> ```

##### Postorder Traversal:

The brute force solution would be to traverse every path starting from every node, but that takes quadratic time. A better solution would be to traverse the tree in postorder and use a modified version of Kadane's algorithm:

```py
def helper(root: "TreeNode") -> "List[int]":
    if not root:
        return [float('-inf'), float('-inf')]   #[max_val in subtree, max_val with root]
    left, right = helper(root.left), helper(root.right)
    left[1], right[1] = max(left[1], 0), max(right[1], 0)
    overall_max = max(left[0], right[0], root.val + left[1] + right[1])
    return [overall_max, root.val + max(left[1], right[1])]

def maxPathSum(root: "TreeNode") -> "int":        
    return self.helper(root)[0] 
```



