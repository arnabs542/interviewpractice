#### Build a Minimum Height BST From a Sorted Array

> Given a sorted array, there are numerous valid BSTs that can be build on the entries in the array. Design an algorithm to build the BST with the minimum possible height.

##### Code:

```
def build_min_height_bst_from_sorted_array(A):
    def bst_helper(start, end):
        if start >= end:
            return None
        mid = (start + end) // 2
        root = BstNode(A[mid])
        root.left = bst_helper(start, mid)
        root.right = bst_helper(mid + 1, end)
        return root
    return bst_helper(0, len(A))
```

##### Explanation:

If we simply iterate through the array, we'll end up with a right skewed tree, since all the values left will be greater than the node that was just constructed. To minimize the height of a BST, we want roughly equal number of nodes in the left and right subtrees, since that'll decrease the height by half each time. 

We construct each node from the middle of the subarray, and recursively build the subtrees. 

Time complexity can be solved by the recurrence relationship: $$\small T(n) = 2T(n/2) + O(1)$$, which resolves to $$\small \mathcal O(n)$$. Another way to think about this is that each node is going to get its own call, with a couple extra calls to to empty arrays. Each call does $$\small \mathcal O(1)$$ work. 

