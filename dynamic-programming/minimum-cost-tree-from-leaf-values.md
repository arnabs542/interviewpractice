#### Minimum Cost Tree From Leaf Values

> Given an array arr of positive integers, consider all binary trees such that:
>
> * Each node has either 0 or 2 children;
> * The values of arr correspond to the values of each leaf in an in-order traversal of the tree.  (Recall that a node is a leaf if and only if it has 0 children.)
> * The value of each non-leaf node is equal to the product of the largest leaf value in its left and right subtree respectively.
>
> Among all possible binary trees considered, return the smallest possible sum of the values of each non-leaf node.  It is guaranteed this sum fits into a `32`-bit integer.
> 
> Example 1:
> ```
> Input: arr = [6,2,4]
> Output: 32
> Explanation:
> There are two possible trees.  The first has non-leaf node sum 36, and the second has non-leaf node sum 32.
>
>     24            24
>    /  \          /  \
>   12   4        6    8
>  /  \               / \
> 6    2             2   4
> ```

##### Solution

The important realization is that the value of root depends only how the values are split; i.e. the value of the root node depends on which leaf values are on its left and which are on its right. Since all we're told is that the leaf values are given to us in an inorder traversal, we know that lie left from right. How we split them is simply up to us - this leads to a recursive solution, which can then be transformed into a dp solution:

```py
def mctFromLeafValues(arr: List[int]) -> int:
    
    memo = {}
    
    def helper(i, j):
        if i >= j:  # 2 <= arr.length
            return 0
        if (i, j) in memo:
            return memo[(i, j)]
        node_val = float('inf')
        # try all different ways of splitting the leaves
        for k in range(i+1, j+1):
            node_val = min(node_val, helper(i, k-1) + helper(k, j) + max(arr[i:k]) * max(arr[k:j+1]))
        memo[(i, j)] = node_val
        return memo[(i, j)]
    
    helper(0, len(arr) - 1)
    return memo[(0, len(arr) - 1)]
```
Note that `i` and `j` are inclusive bounds, while `k` is used as an exclusive bound for list splicing. Therefore the left subtree needs to be called with `k-1` to match the consistency of `j`.

Running time is $\small \mathcal O(n^2)$.

However, this problem can actually be solved optimally in $\small \mathcal O(n)$ in a greedy solution. There are a few key points that leads us to this realization:

1. The leaf node with the smaller value is used only once and then discard. In the above example, regardless of whether we pair `2` with `4` or with `6`, it is used only once.
2. Since the leaves are give in inorder traversal, the smaller node must be combined with its immediate left or right neighbor (if they exist).

The general optimization strategy is to place the largest nodes as high as possible so they're used as infrequently as possible. This allows us to reframe the problem as such: 

> From a given an array `A`, pick two neighbors in the array: `a` and `b`. We can remove the smaller one at the cost of `a*b`. What is the minimum cost to remove the whole array until only one value is left?

We would like to minimize `b` (given that `a` is the smaller number). Since `b` has two candidates - the leaf on the left and the leaf on the right, we simply do `a * min(left, right)`.

```py
def mctFromLeafValues(arr: List[int]) -> int:
    
    stack = [float('inf')]
    res = 0
    for a in arr:
        while stack and stack[-1] <= a:
            mid = stack.pop()
            res += mid*(min(stack[-1], a))
        stack.append(a)
    
    # Stack is now monotonically increasing
    while len(stack) > 2:
        right = stack.pop()
        res += right * stack[-1]
    
    return res
```