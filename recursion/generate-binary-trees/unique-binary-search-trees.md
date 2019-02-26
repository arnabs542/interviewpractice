#### Unique Binary Search Trees

> Given _n_, how many structurally unique **BST's** \(binary search trees\) that store values 1 ... _n_?
>
> **Example:**
>
> ```
> Input: 3
>
> Output: 5
>
> Explanation:
>
> Given n = 3, there are a total of 5 unique BST's:
>
>    1         3     3      2      1
>     \       /     /      / \      \
>      3     2     1      1   3      2
>     /     /       \                 \
>    2     1         2                 3
> ```

##### Naive Recursion \(TLE\):

```py
def numTrees(n: int) -> int:       
    if n <= 2:
        return n
    count = 0
    for left in range(n):
        right = n - left - 1
        left_count, right_count = self.numTrees(left), self.numTrees(right)
        count += max(1, left_count) * max(1, right_count)
    return count
```

This is almost the same problem as the parent problem. Here we're asked to simply return the number of unique trees, instead of what the trees actually are.

We again break the computation into finding the number of left and right subtrees. Since `left_count, right_count` could be 0, we need to bound it to 1. Once we find the number of subtrees, we multiply to get the number of total trees we can have with the number of left and right nodes in the subtrees.

This results in the recurrence formula of $$\small T(n) = \sum_{i=0}^{n-1} T(i)*T(n-i-1)$$, which quickly times out. 

##### Memoization:

```py
class Solution:
    memo = {}
    def numTrees(self, n: int) -> int:       
        if n <= 2:
            return n
        if n in self.memo:
            return self.memo[n]
        count = 0
        for left in range(n):
            right = n - left - 1
            left_count, right_count = self.numTrees(left), self.numTrees(right)
            count += max(1, left_count) * max(1, right_count)
        self.memo[n] = count
        return count
```



