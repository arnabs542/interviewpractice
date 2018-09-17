#### The Range Lookup Problem

> Write a program that takes as input a BST and an interval and returns the BST keys that lie in the interval.

##### Code \(Iteration\):

```py
def range_lookup_in_bst(tree, interval):
    res = []

    def inorder(tree):
        if not tree:
            return
        inorder(tree.left)
        if interval.left <= tree.data <= interval.right:
            res.append(tree.data)
        inorder(tree.right)

    inorder(tree)
    return res
```

##### Explanation:

The brute force solution is to simply iterate over the entire tree and append all nodes with values contained in the interval. The time complexity is $$\small \mathcal O(n)$$, and space is bounded by recursion stack: $$\small \mathcal O(h)$$. The solution is not ideal because we don't take advantage of the relationship between the nodes and their children. For example, suppose `tree.data < interval.left` - there is no reason to traverse down the left subtree at all, since all values will be less than equal to the root value.

##### Code \(Optimized\):

```py
def range_lookup_in_bst(tree, interval):

    def range_look_up_helper(tree):
        if not tree:
            return
        if interval.left <= tree.data <= interval.right:
            # tree.data lies in interval
            range_look_up_helper(tree.left)
            result.append(tree.data)
            range_look_up_helper(tree.right)
        elif tree.data < interval.left:
            # tree.data too small
            range_look_up_helper(tree.right)
        else:
            # tree.data too large
            range_look_up_helper(tree.left)

    result = []
    range_look_up_helper(tree)
    return result
```

##### Explanation:

The time complexity is still roughly bounded by $$\small \mathcal O(n)$$, but that's actually the best we can do - in the event the interval given covers all values in the tree, we have no choice but to iterate over every single node.

However, the extra conditionals we put in the traversal will help us discard entire subtrees as soon as we can do so. If the current root value is outside of the interval, then we can automatically discard either the left or subtree depending on which side of the interval the root lies in.

