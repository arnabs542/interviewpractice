#### Find the First Key Greater than a Given Value in a BST

> Write a program that takes as input a BST and a value, and returns the first key that would appear in an inorder traversal which is greater than the input value.

##### Iteration:

```
def find_first_greater_than_k(tree, k):
    stack = []
    cur = tree
    while stack or cur:
        if cur:
            stack.append(cur)
            cur = cur.left
        else:
            cur = stack.pop()
            if cur.data > k:
                return cur
            cur = cur.right
    return None
```

##### Explanation:

We traverse the tree inorder iteratively, using a stack. We return the first node whose value is greater than the given value. Running time is $\small \mathcal  O(n)$, space is $\small \mathcal O(h)$. However, this solution doesn't utilize the BST property effectively. 

##### Recursion:

```py
def find_first_greater_than_k(tree, k):
    subtree, first_so_far = tree, None
    while subtree:
        if subtree.data > k:    # Current node value is greater than k, all values on right is greater than k
            first_so_far = subtree
            subtree = subtree.left
        else:   # Current node value is smaller than k, all values on left side is smaller than k
            subtree = subtree.right
    return first_so_far
```

##### Explanation:

If we take advantage of the BST property, we can solve the problem much quicker. If a current node is smaller than/equal to the given value, there is no point in searching in the left subtree, since all values must be smaller than or equal to the current node. If the current node is greater than the BST, then we don't need to search in the right subtree, since all values must be larger than or equal to the current node, which means either the current node is the first node greater than $\small k$, or the first node is in the left subtree. 

Running time is reduced to $\small \mathcal O(h)$, space is reduced to $\small \mathcal O(1)$.

