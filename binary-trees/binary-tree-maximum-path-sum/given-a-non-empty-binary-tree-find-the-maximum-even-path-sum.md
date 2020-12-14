#### Binary Tree Maximum Even Path Sum

> Given a non-empty binary tree, find the maximum **even** path sum.
>
> **Example 1:**
>
> ```
> Input:
>
>        10
>      /    \
>     2      5
>    /  \      \
>   1   101    13
>
> Output: 118
> Explanation: 101 + 2 + 10 + 5 = 118
> ```

##### Postorder:

```py
def helper(root):
    if not root:
        #[max even in tree, max even with root, max odd with root]
        return [float('-inf'), float('-inf'), float('-inf')]   
    left, right = helper(root.left), helper(root.right)

    # Get best even and odd path with current root
    new_root_maxes = [root.val + left[1], root.val + left[2], root.val + right[1], root.val + right[2]]
    odd_max = even_max = float('inf')

    for n in new_root_maxes:
        if n % 2:   
            odd_max = max(odd_max, n)
        else:   
            even_max = max(even_max, n)


    # Get best overall even
    new_overall_maxes = [    
                            root.val
                            root.val + left[1] + right[1], 
                            root.val + left[2] + right[1], 
                            root.val + right[1] + left[2], 
                            root.val + right[2] + left[2],
                            root.val + left[1],
                            root.val + left[2],
                            root.val + right[1],
                            root.val + right[2]
                         ]

    overall_max = float('-inf')
    for n in new_overall_maxes:
        if n % 2 == 0:
            overall_max = max(overall_max, n)

    return [overall_max, even_max, odd_max]            

def maxPathSum(root: TreeNode) -> int:        
    return helper(root)[0]
```

Essentially the same idea as the previous solution, except we need to return both the maximum even path and the maximum odd path ending at the current node.

As we get the left and right subtree results, we first calculate the maximum even and odd paths that end at the current node. Since these will be the paths that we propagate up the chain, we can only pick either the left or right paths.

We then try to build a global maximum with the current node, which means we need to add the current root value to all combinations of left and right subtree paths.

Overall runtime is $\small \mathcal O(n)$.

Note that the above solution assumes positive node values. In the case of negative node values, we need to consider 

