#### Binary Tree Coloring Game - LC 1145

> Two players play a turn based game on a binary tree.  We are given the root of this binary tree, and the number of nodes n in the tree.  `n` is odd, and each node has a distinct value from `1` to `n`.
>
> Initially, the first player names a value `x` with `1 <= x <= n`, and the second player names a value `y` with `1 <= y <= n` and `y != x`.  The first player colors the node with value `x` red, and the second player colors the node with value `y` blue.
>
> Then, the players take turns starting with the first player.  In each turn, that player chooses a node of their color (red if player `1`, blue if player `2`) and colors an uncolored neighbor of the chosen node (either the left child, right child, or parent of the chosen node.)
>
> If (and only if) a player cannot choose such a node in this way, they must pass their turn.  If both players pass their turn, the game ends, and the winner is the player that colored more nodes.
>
> You are the second player.  If it is possible to choose such a `y` to ensure you win the game, return true.  If it is not possible, return false.
>
> Example 1:
> 
> ![](../assets/binary-tree_coloring_game.png)
> ```> 
> Input: root = [1,2,3,4,5,6,7,8,9,10,11], n = 11, x = 3
> Output: true
> Explanation: The second player can choose the node with value 2.
> ```
>
> Constraints:
>
> * `root` is the root of a binary tree with `n` nodes and distinct node values from `1` to `n`.
> * `n` is odd.
> * `1 <= x <= n <= 100`

##### Solution

While this problem initially seems difficult, it's actually quite simple. The game will actually be decided immdiedately after the first move is made. 

In the above example, the first player chooses `3`. Every node has at most three neighbors: a parent, a left child, and a right child. If player 2 chooses `1`, he cuts off the parent node and all other "cousin" nodes. If player 2 chooses `6`, he cuts of the left subtrees. Choosing `7` cuts off the right subtree. 

However, by making a move, player 2 has also limited all his future options. By choosing a child node, he won't be able to use all of the nodes in the parent path or the right subtree path. By choosing the parent path, he won't be able to use any node in the children. Thus, after two moves, the total amount of of nodes the players can choose are already set. 

The problem then simply breaks down into how many nodes are in the parent branch, the left subtree, and the right subtree. If any of these options is greater than the other two options combined, then player 2 can win the game:

```py
def btreeGameWinningMove(self, root: TreeNode, n: int, x: int) -> bool:
    
    self.node_x = None
    
    # Count above
    def count_above(node, x):
        if not node:
            return 0
        if node.val == x:
            self.node_x = node
            return 0
        return 1 + count_above(node.left, x) + count_above(node.right, x)
    
    # Count below
    def count_nodes(node):
        if not node:
            return 0
        return 1 + count_nodes(node.left) + count_nodes(node.right)
    
    above = count_above(root, x)
    left, right = count_nodes(self.node_x.left), count_nodes(self.node_x.right)
    
    return (above > left + right + 1) or (left > above + right + 1) or (right > above + left + 1)
```

Running time and space usage is the same as any tree traversal.