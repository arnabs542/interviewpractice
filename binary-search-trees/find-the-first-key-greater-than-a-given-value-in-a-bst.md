#### Find the First Key Greater than a Given Value in a BST

> Write a program that takes as input a BST and a value, and returns the first key that would appear in an inorder traversal which is greater than the input value.

##### Code \(Iteration\):

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

We traverse the tree inorder iteratively, using a stack. We return the first node whose value is greater than the given value. 

