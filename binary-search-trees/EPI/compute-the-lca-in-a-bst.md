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

##### Code \(Iterative\):

```py
# Input nodes are nonempty and the key at s is less than or equal to that at b.
def find_LCA(tree, s, b):
    while tree.data < s.data or tree.data > b.data:
        # Keep searching since tree is outside of [s,b]
        while tree.data < s.data:
            tree = tree.right   # LCA must be in tree's right child
        while tree.data > b.data:
            tree = tree.left    # LCA must be in tree's left child
    # Now, s.data <= tree.data && tree.data <= b.data
    return tree
```

##### Explanation:

We can actually improve on the above solution by making the given node `s` always smaller than `b`.

 The goal remains the same: make one of the nodes smaller than the root and the other equal to the root, or make one of the nodes the root. By always setting one node as the smaller one, we can use constant space by performing the search iteratively. Time complexity remains the same. 

