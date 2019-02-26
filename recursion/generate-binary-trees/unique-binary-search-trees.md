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

We see there is a lot of overlap between the work being done. For example, suppose $$\small n=3$$. We will calculate the following scenarios:

```
left    right
 0        2
 1        1
 2        1
```

The results of `left = 0, right = 2` and `left = 2, right = 0` should be symmetric. Thus, we simply record all the work we've already done. The running time should be reduced to $$\small \mathcal O(n)$$, since each left and right subtree number should be calculated once.

##### Bottom Up:

```py
def numTrees(self, n: int) -> int:
    # dp[n] stores the number of trees with n nodes
    dp = [0] * (n+1)
    dp[0] = dp[1] = 1

    # Iterate through the total number of nodes
    for nodes in range(2, n + 1):
        # For each value, assign it as the root
        for root in range(nodes + 1):
            # For a given root value r, there are r - 1 numbers that belong on the left
            # and nodes - r numbers that belong on the right
            dp[nodes] +=  dp[root - 1] * dp[nodes - root]

    return dp[-1]
```

The idea of the above solution is that we first iterate through the total number of nodes from 1 ~ n. For each number $$\small i$$, we iterate from 1 ~ $$\small i$$ using $$\small r$$ and place $$\small r$$ as the root value. For each $$\small r$$, there are $$\small r-1$$ nodes that could be place on the left, and $$\small i - r$$ nodes on the right.

Taking 1~n as root respectively:

* 1 as root: \# of trees = F\(0\) \* F\(n-1\)  // F\(0\) == 1
* 2 as root: \# of trees = F\(1\) \* F\(n-2\) 
* 3 as root: \# of trees = F\(2\) \* F\(n-3\)
* ...
* n-1 as root: \# of trees = F\(n-2\) \* F\(1\)
* n as root:   \# of trees = F\(n-1\) \* F\(0\)

The solution above uses $$\small \mathcal O(n^{2})$$ time and $$\small \mathcal O(n)$$ space.

