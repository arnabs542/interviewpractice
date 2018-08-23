#### Find Duplicate Subtrees

> Given a binary tree, return all duplicate subtrees. For each kind of duplicate subtrees, you only need to return the root node of any **one** of them.
>
> Two trees are duplicate if they have the same structure with same node values.
>
> **Example 1:**
>
> ```
>         1
>        / \
>       2   3
>      /   / \
>     4   2   4
>        /
>       4
> ```
>
> The following are two duplicate subtrees:
>
> ```
>       2
>      /
>     4
> ```
>
> and
>
> ```
> 4
> ```
>
>  Therefore, you need to return above trees' root in the form of a list: \[\[4\],\[2,4\]\].

My initial approach was to perform a preorder traversal, and each time we encountered a root with a value that we had already encountered, perform DFS on the two nodes as roots to see if they are duplicate subtrees. However, the time complexity for this is extremely bad, since if there are a lot of nodes with the same value, we'll end up iterating over the tree every time. 

