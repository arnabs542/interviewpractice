#### Find the Second Largest Element in a Binary Search Tree

> Given a binary search tree, find and return the second largest element.

##### Solution

Since an inorder traversal of a BST produces a sorted array, we could simply traverse the tree and return the 2nd to last element. To speed this up, we could perform a reversed inorder traversal and break as soon as we have the second largest element:

```py
def second_largest(root, vals):
    if not root or vals[1] is not None:
        return
    second_largest(root.right, vals)
    if vals[0] is None:
        vals[0] = root.val
    elif vals[1] is None:
        vals[1] = root.val
    second_largest(root.left, vals)

vals = [None, None]
return second_largest(root, vals)
```

With the early termination, this will run in $\small \mathcal O(h)$ time and take $\small \mathcal O(h)$ space thanks to recursion stack. 

However, we can actually do better in terms of space. Let us first think of how we would find the **largest** element in a BST. This would simply be the rightmost element:

```py
def find_largest(root_node):

    if root_node.right:
        return find_largest(root_node.right)
    return root_node.value
```

It would be natural to think that the second largest element is simply the parent of the largest element. This would be true in a balanced tree like below:

```
         ( 5 )
        /     \
      (3)     (8)
     /  \     /  \
   (1)  (4) (7)  (9)
```

However, this assumption doesn't hold when the largest element itself has a left subtree:

```
         ( 5 )
        /     \
      (3)     (8)
     /  \     /  \
   (1)  (4) (7)  (12)
                 /
               (10)
               /  \
             (9)  (11)
```

But this situation is also easy to adjust to - in the event that the largest element has a left subtree, the second largest element is simply the largest element in that left subtree.

```py
def find_largest(root_node):
    current = root_node
    while current:
        if not current.right:
            return current.value
        current = current.right


def find_second_largest(root_node):
    if (root_node is None or
            (root_node.left is None and root_node.right is None)):
        raise ValueError('Tree must have at least 2 nodes')

    current = root_node
    while current:
        # Case: current is largest and has a left subtree
        # 2nd largest is the largest in that subtree
        if current.left and not current.right:
            return find_largest(current.left)

        # Case: current is parent of largest, and largest has no children,
        # so current is 2nd largest
        if (current.right and
                not current.right.left and
                not current.right.right):
            return current.value

        current = current.right
```

This still takes $\small \mathcal O(h)$ time, but uses only $\small \mathcal O(1)$ space.

Notice that BSTs often allow us to search for an element in an iterative manner, thanks to its ordered nature. This makes it better than normal binary trees.