##### Flip Binary Tree To Match Preorder Traversal

Given a binary tree with `N` nodes, each node has a different value from `{1, ..., N}`.

A node in this binary tree can be _flipped_ by swapping the left child and the right child of that node.

Consider the sequence of `N` values reported by a preorder traversal starting from the root.  Call such a sequence of `N` values the _voyage_ of the tree.

\(Recall that a _preorder traversal_ of a node means we report the current node's value, then preorder-traverse the left child, then preorder-traverse the right child.\)

Our goal is to flip the **least number** of nodes in the tree so that the voyage of the tree matches the `voyage` we are given.

If we can do so, then return a list of the values of all nodes flipped.  You may return the answer in any order.

If we cannot do so, then return the list `[-1]`.

**Example 1:**

![](/assets/flip_tree_preorder.png)

```
Input: root = [1,2,3], voyage = [1,3,2]
Output: [1]
```

##### Preorder Traversal:

```py
def flipMatchVoyage(self, root, voyage):
    flips = []
    self.idx = 0

    def preorder(root, voyage):

        if not root or self.idx == len(voyage):
            return True            
        if root.val != voyage[self.idx]:
            return False
        self.idx += 1
        if not root.left:
            return preorder(root.right, voyage)
        else:
            if root.left.val != voyage[self.idx]:
                root.left, root.right = root.right, root.left
                flips.append(root.val)
            return preorder(root.left, voyage) and preorder(root.right, voyage)

    if preorder(root, voyage):
        return flips
    return [-1]
```

The overall idea is quite simple, because we're assured that each node has a distinct value. Therefore, we don't need to worry about the case where both children have the same value as the next value in the traversal.

We simply go through the tree in a preorder traversal and attempt to match our voyage:

1.  We use a global integer idx to indicate the next index in the voyage. 
2. If current `node == null`, it's fine, we return `true`.  
3. If current `node.val != v[i]`, it doesn't match wanted value, return `false`.
4. If left child exist but don't have wanted value, flip it with right child.



