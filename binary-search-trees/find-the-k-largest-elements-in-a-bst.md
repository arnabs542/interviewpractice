#### Find the k Largest Elements in a BST

Write a program that takes as input a BST and an integer $$\small k$$, and returns the $$\small k$$ largest elements in the BST in decreasing order.

##### Code \(Iteration\):

```py
def find_k_largest_in_bst(tree, k):
    k_largest_elements = []
    stack, cur = [], tree
    while stack or cur:
        if cur:
            stack.append(cur)
            cur = cur.right
        else:
            cur = stack.pop()
            k_largest_elements.append(cur.data)
            if len(k_largest_elements) == k:
                return k_largest_elements
            cur = cur.left
    return k_largest_elements
```

##### Code \(Recursion\):

```py
def find_k_largest_in_bst(tree, k):
    k_largest_elements = []
    def find_k_largest_in_bst_helper(tree):
        if not tree:
            return
        find_k_largest_in_bst_helper(tree.right)
        if len(k_largest_elements) < k:
            k_largest_elements.append(tree.data)
            find_k_largest_in_bst_helper(tree.left)
    find_k_largest_in_bst_helper(tree)
    return k_largest_elements
```

Explanation:

The key concept here is that an inorder traversal of a BST results in a sorted array. Therefore we simply need to do a reverse inorder traversal to get the k largest elements. We can do this either iteratively or recursively. The time is bounded by $$\small \mathcal O(h + k)$$, because we need to traverse all the way down to the right most leaf node and then $$\small k$$ more times. Space is bounded by the recursive stack, which is $$\small \mathcal O(h)$$.

