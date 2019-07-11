#### Delete Node and Return Forest

> Given the root of a binary tree, each node in the tree has a distinct value.
>
> After deleting all nodes with a value in to_delete, we are left with a forest (a disjoint union of trees).
>
> Return the roots of the trees in the remaining forest.  You may return the result in any order.
>
> Example 1:
> ```
> Input: root = [1,2,3,4,5,6,7], to_delete = [3,5]
> Output: [[1,2,null,4],[6],[7]]
> ```
> Constraints:
>
> * The number of nodes in the given tree is at most 1000.
> * Each node has a distinct value between 1 and 1000.
> * to_delete.length <= 1000
> * to_delete contains distinct values between 1 and 1000.

##### Solution

As with all tree problems, our first thought should be recursion. 

The recursive relationship is as follows:

1. If a node is `None`, return `None`
2. Prune the left and right subtrees
3. If the node itself is to be pruned, append its left and right children (if they exist) to the result array and return `None`
4. If the root if not pruned, return it as well. Otherwise, return the array 

```py
def delNodes(root: "TreeNode", to_delete: "List[int]") -> "List[TreeNode]":
    res = []
    
    def prune_tree(root, to_delete, res):
        if not root:
            return None
        root.left, root.right = prune_tree(root.left, to_delete, res), prune_tree(root.right, to_delete, res)
        if root.val in to_delete:
            if root.left:
                res.append(root.left)
            if root.right:
                res.append(root.right)
            return None
        return root
    
    pruned_tree = prune_tree(root, set(to_delete), res)
    if pruned_tree:
        res.append(pruned_tree)
    return res
```

Running time is $\small \mathcal O(n)$. Space is same in the case of an unbalanced tree.