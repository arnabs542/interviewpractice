#### Compute the LCA in a BST

> Since a BST is a specialized binary tree, the notion of lowest common ancestor holds for BSTs too. In general, computing the LCA of two nodes in a BST is no easier than computing the LCA in a binary tree, since structurally a binary tree can be viewed as a BST where all the keys are equal. However, when the keys are distinct, it is possible to improve on the LCA algorithms for binary trees. 
>
> Design an algorithm that takes as input a BST and tow nodes, and returns the LCA of the two nodes.

##### Code:

```py
def find_LCA(tree, s, b):
    if not tree:
        return None
    if s.data < tree.data and b.data > tree.data:
        return tree
    if tree == s or tree == b:
        return tree
    if s.data < tree.data and b.data < tree.data:
        return find_LCA(tree.left, s, b)
    return find_LCA(tree.right, s, b)
```

##### Explanation:

We take advantage of the relative ordering between the root of a BST and its node values to throw away half the tree each time. Running time is $$\small \mathcal O(h)$$, since we descent a level each time. Space is bounded by recursive call stack, also $$\small \mathcal O(h)$$.

