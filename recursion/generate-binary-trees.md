#### Generate Binary Trees

> Write a program which returns all distinct binary trees with a specified number of nodes. For example, if the number of nodes is specified to be 3, return 5:
>
> ```
> 0         0         0         0    0
>  \         \       / \       /    /
>   0         0     0   0     0    0
>    \       /               /      \
>     0     0               0        0
> ```
>
> Note that we're only counting distinct shapes as distinct trees, i.e.
>
> ```
> 1       1
>  \       \
>   2       3
>    \       \
>     3       2
> ```
>
> counts as the same tree in this scenario.

##### Recursion:

```py
def generate_all_binary_trees(num_nodes: "int") -> "List[BinaryTreeNode]":
    if num_nodes == 0:
        return [0]

    results = []
    for left_subtree_nodes in range(num_nodes):
        right_subtree_nodes = num_nodes - left_subtree_nodes - 1
        left_subtrees = generate_all_binary_trees(left_subtree_nodes)
        right_subtrees = generate_all_binary_trees(right_subtree_nodes)
        results += [BinaryTreeNode(0, l, r) for l in left_subtrees for r in right_subtrees]
    return results
```

The recursion pattern comes from the fact that the total number of nodes is bounded by the given number $$\small n$$. Thus, if the left subtree has $$\small l$$ nodes, then the right subtree has $$\small n - l - 1$$ nodes \(one node is for the root\). We know that $$\small l$$ goes from 0 to $$\small n-1$$, which means that each recursive call will reduce the size of the input from its parent call. The base case is when $$\small n$$ is 0, since there is one possible tree - the empty tree.

The number of calls $$\small C(n)$$ to the recursive function satisfies the recurrence $$\small C(n) = \sum_{i=1}^{n} C(n-i)C(i-1)$$. The quantity $$\small C(n)$$ is called the nth Catalan number, and equals $$\small \frac{(2n)!}{n!(n+1)!}$$. Catalan numbers appear in numerous types of combinatorial problems.

