#### Unique Binary Search Trees II

> Given an integer _n_, generate all structurally unique **BST's** \(binary search trees\) that store values 1 ... _n_.
>
> **Example:**
>
> ```
> Input: 3
>
> Output:
> [
>   [1,null,3,2],
>   [3,2,null,1],
>   [3,1,null,null,2],
>   [2,1,3],
>   [1,null,2,null,3]
> ]
>
> Explanation: The above output corresponds to the 5 unique BST's shown below:
>
>    1         3     3      2      1
>     \       /     /      / \      \
>      3     2     1      1   3      2
>     /     /       \                 \
>    2     1         2                 3
> ```

##### Recursion:

```py
def generateTrees(n: int) -> List[TreeNode]:
    
    if n == 0:
        return []
    
    def helper(start, end):
        
        if start > end:
            return [None]
        if start == end:
            return [TreeNode(start)]
        
        res = []
        for i in range(start, end+1):
            left_trees = helper(start, i-1)
            right_trees = helper(i+1, end)
            for left in left_trees:
                for right in right_trees:
                    root = TreeNode(i)
                    root.left, root.right = left, right
                    res.append(root)
        return res
    
    return helper(1, n)
```

The above solution is based on the property that \[1:n\] is the in-order traversal for any BST with nodes valued 1 through n. If we pick the i-th node as the root, the left subtree will contain values \[1:i-1\], and the right subtree will contain values \[i+1:n\]. We then build the tree recursively by splitting the node value range until we hit one of the base cases.

Running time is still based on the Catalan numbers sequence. 

