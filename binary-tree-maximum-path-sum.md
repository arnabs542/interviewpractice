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
>   1
>  / \
> 2   3
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
>  /  \
> 15   7
>
> Output: 42
> ```

##### Code:

```py
class Solution:    
    def maxPathSum(self, root):
        self.max_path = float("-inf")
        def helper(root):
            if not root:
                return float("-inf")
            max_left_path = max(helper(root.left), 0)
            max_right_path = max(helper(root.right), 0)
            self.max_path = max(self.max_path, root.val + max_left_path + max_right_path)
            return max(max_left_path, max_right_path) + root.val
        helper(root)
        return self.max_path
```

##### Explanation:

The brute-force solution would be to test all paths, and see which one has the greatest sum. Since we're told that the path can't be empty, that means the maximum path needs to start with some node as its root. Therefore, we could preorder traverse the tree, and treat each node as the possible root of the largest path.

The problem is we iterate over subtrees over and over again. In the case of a skewed tree, we essentially run a nested for loop, taking $$\small \mathcal O(n^{2})$$ time. Instead, we need to think about the problem from a bottom up solution. Suppose we know the maximum path on the left side and maximum path on the right side. Then we simply need to see if either, both, or neither added to the current root value is the new maximum path sum.

The postorder solution instead takes only $$\small \mathcal O(n)$$ time and $$\small \mathcal O(h)$$ space complexity. The problem is really a just more advanced version of Kadane's algorithm. 

