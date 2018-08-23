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
> Therefore, you need to return above trees' root in the form of a list: \[\[4\],\[2,4\]\].

My initial approach was to perform a preorder traversal, and each time we encountered a root with a value that we had already encountered, perform DFS on the two nodes as roots to see if they are duplicate subtrees. However, the time complexity for this is extremely bad, since if there are a lot of nodes with the same value, we'll end up iterating over the tree every time.

##### Code:

```py
def findDuplicateSubtrees(root):

    paths = collections.defaultdict(list)

    def serialize(root):
        if not root:
            return "#"
        path = "{0} {1} {2}".format(root.val, serialize(root.left), serialize(root.right))
        paths[path].append(root)
        return path

    serialize(root)

    return [v[0] for k,v in paths.items() if len(v) > 1]
```

##### Explanation:

The idea is to serialize the path from each root to a leaf, and to use that as as the key instead of note values themselves. Duplicate subtrees will have the same paths, and once we traverse the tree completely, we simply check for paths with more than one root. Pay attention that we must use either preorder or postorder traversal - inorder traversal with null markers will result in identical paths for different trees \(try some examples\). 

Overall runtime should be $$\small \mathcal O(n)$$ from the tree traversal, however additional time maybe be required depending on string construction. 

