#### Reconstruct a BST From Traversal Data

> Suppose you are given the keys of a BST in preorder traversal, and all keys are distinct. Can you reconstruct the BST from the sequence? Solve the same problem for inorder and postorder traversal sequences.

##### Code \(Unoptimized\):

```py
def rebuild_bst_from_preorder(preorder_sequence):
    if not preorder_sequence:
        return None
    transition_point = next((i for i, a in enumerate(preorder_sequence) if a > preorder_sequence[0]), 
                            len(preorder_sequence))
    root = BstNode(preorder_sequence[0])
    root.left = rebuild_bst_from_preorder(preorder_sequence[1:transition_point])
    root.right = rebuild_bst_from_preorder(preorder_sequence[transition_point:])
    return root
```

##### Explanation:

A preorder traversal of a tree goes `[root, preorder(root.left), preorder(root.right)]`. Since all values on the left of a BST must be smaller than the root, and all values on the right of a BST must be larger than the root, we simply need to find the transition point where the first element larger than the root value appears, split the array, and build the subtrees recursively. The problem with the above implementation is that we constantly iterate over the array, and this takes a lot of time. Suppose the tree is left-skewed. Each step of the recursive call would result in us doing only 1 unit of work, giving us the recurrence relationship $$\small T(n) = T(n-1) + \mathcal O(1)$$. This solves to a running time of $$\small O(n^{2})$$. In the event of a right-skewed tree, running time is $$\small \mathcal O(n)$$.

> `next(iterator[, default]`
>
> Retrieve the next item from the _iterator_ by calling its [`__next__()`](https://docs.python.org/3/library/stdtypes.html#iterator.__next__) method. If _default_ is given, it is returned if the iterator is exhausted, otherwise [`StopIteration`](https://docs.python.org/3/library/exceptions.html#StopIteration) is raised.

##### Code \(Optimized\):

```py
def rebuild_bst_from_preorder(preorder_sequence):
    def rebuild_bst_from_preorder_helper(lower_bound, upper_bound):
        if root_idx[0] == len(preorder_sequence):
            return None
        root = preorder_sequence[root_idx[0]]
        if not lower_bound <= root <= upper_bound:
            return None
        root_idx[0] += 1
        left_subtree = rebuild_bst_from_preorder_helper(lower_bound, root)
        right_subtree = rebuild_bst_from_preorder_helper(root, upper_bound)
        return BstNode(root, left_subtree, right_subtree)
    root_idx = [0]
    return rebuild_bst_from_preorder_helper(float('-inf'), float('inf'))
```

##### Explanation:

Once again, we take advantage of the ordering between BST nodes to help us solve the problem quicker. We begin by looking at a which node we're on, and whether it belongs in the current range we're trying to build. 

We use a root\_idx pointer to traverse through the preorder sequence. 

