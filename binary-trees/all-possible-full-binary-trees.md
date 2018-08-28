#### All Possible Full Binary Trees

> A _full binary tree_ is a binary tree where each node has exactly 0 or 2 children.
>
> Return a list of all possible full binary trees with `N` nodes.  Each element of the answer is the root node of one possible tree.
>
> Each `node` of each tree in the answer **must** have `node.val = 0`.
>
> You may return the final list of trees in any order.**Example 1:**
>
> ```
> Input: 7
> Output: [[0,0,0,null,null,0,0,null,null,0,0],[0,0,0,null,null,0,0,0,0],[0,0,0,0,0,0,0],[0,0,0,0,0,null,null,null,null,0,0],[0,0,0,0,0,null,null,0,0]]
> Explanation:
> ```
>
> ![](/assets/full_trees.png)

##### Code:

```py
class Solution(object):
    memo = {0: [], 1: [TreeNode(0)]}

    def allPossibleFBT(self, N):
        if N not in Solution.memo:
            ans = []
            for x in range(N):
                y = N - 1 - x
                for left in self.allPossibleFBT(x):
                    for right in self.allPossibleFBT(y):
                        bns = TreeNode(0)
                        bns.left = left
                        bns.right = right
                        ans.append(bns)
            Solution.memo[N] = ans
        return Solution.memo[N]
```

##### Explanation:

The idea is to use memoization. We know the base cases when $$\small N = 0$$ and $$\small N = 1$$ immediately - those will be our seeds. We then frame the problem recursively:

Let $$\small FBT(N)$$ be the list of all possible full binary trees with $$\small N$$ nodes. Every full binary tree $$\small T$$ with 3 or more nodes, has 2 children at its root. Each of those children `left` and `right` are themselves full binary trees.

Thus, for $$\small N \geq 3$$, we can formulate the recursion: $$\small FBT(N) = $$ \[All trees with left child $$\small FBT(x)$$ and right child $$\small FBT(N - 1- x)$$, for all $$\small x$$\]. 

We cache the results to avoid doin extra work. 



