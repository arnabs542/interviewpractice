#### Distribute Coins in Binary Tree

> Given the `root` of a binary tree with `N` nodes, each `node` in the tree has `node.val` coins, and there are `N` coins total.
>
> In one move, we may choose two adjacent nodes and move one coin from one node to another.  \(The move may be from parent to child, or from child to parent.\)
>
> Return the number of moves required to make every node have exactly one coin.
>
> **Example 1:**
>
> ![](https://assets.leetcode.com/uploads/2019/01/18/tree1.png)
>
> ```
> Input: [3,0,0]
> Output: 2
> Explanation: From the root of the tree, we move one coin to its left child, and one coin to its right child.
> ```
>
> **Example 2:**
>
> ![](https://assets.leetcode.com/uploads/2019/01/18/tree2.png)
>
> ```
> Input: [0,3,0]
> Output: 3
> Explanation: From the left child of the root, we move two coins to the root [taking two moves].  
> Then, we move one coin from the root of the tree to the right child.
> ```

##### Postorder traversal:

```py
class Solution(object):
    def distributeCoins(self, root):
        self.ans = 0

        def dfs(node):
            if not node: return 0
            L, R = dfs(node.left), dfs(node.right)
            self.ans += abs(L) + abs(R)
            return node.val + L + R - 1

        dfs(root)
        return self.ans
```

If the leaf of a tree has 0 coins \(an excess of -1 from what it needs\), then we should push a coin from its parent onto the leaf. If it has say, 4 coins \(an excess of 3\), then we should push 3 coins off the leaf. In total, the number of moves from that leaf to or from its parent is `excess = Math.abs(num_coins - 1)`. Afterwards, we never have to consider this leaf again in the rest of our calculation.

 We can use the above fact to build our answer. Let `dfs(node)` be the _excess_ number of coins in the subtree at or below this `node`: namely, the number of coins in the subtree, minus the number of nodes in the subtree. Then, the number of moves we make from this node to and from its children is `abs(dfs(node.left)) + abs(dfs(node.right))`. After, we have an excess of `node.val + dfs(node.left) + dfs(node.right) - 1` coins at this node.

Runtime is $$\small \mathcal O(n)$$, space is $$\small \mathcal O(h)$$.

