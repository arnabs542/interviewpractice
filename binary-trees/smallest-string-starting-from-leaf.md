#### Smallest String Starting From Leaf

> Given the `root` of a binary tree, each node has a value from `0` to `25` representing the letters `'a'` to `'z'`: a value of `0` represents `'a'`, a value of `1` represents `'b'`, and so on.
>
> Find the lexicographically smallest string that starts at a leaf of this tree and ends at the root.
>
> _\(As a reminder, any shorter prefix of a string is lexicographically smaller: for example, _`"ab"`_ is lexicographically smaller than _`"aba"`_.  A leaf of a node is a node that has no children.\)_
>
> **Example 1:**
>
> ![](/assets/string_from_leaf_1.png)
>
> ```
> Input: [0,1,2,3,4,3,4]
> Output: "dba"
> ```
>
> **Example 2:**
>
> ![](/assets/string_from_leaf_2.png)
>
> ```
> Input: [25,1,3,1,3,0,2]
> Output: "adz"
> ```
>
> **Example 3:**
>
> ![](/assets/string_from_leaf_3.png)
>
> ```
> Input: [2,2,1,null,1,0,null,0]
> Output: "abc"
> ```

##### Postorder:

```py
def smallestFromLeaf(root: 'TreeNode') -> 'str':

    def helper(root):
        if not root:
            return ""
        left, right, = helper(root.left), helper(root.right)
        left, right = left + chr(root.val + ord('a')), right + chr(root.val + ord('a'))
        if root.left and root.right:
            return min(left, right)
        return left if not root.right else right

    return helper(root)
```

We pass the string from each node up to its parent, which then picks the smallest of the two strings from its children and appends its own value to it. The traversal takes $$\small \mathcal O(n)$$ time, but since Python strings are immutable, every time we add a character to a new string, we need to recreate the entire string. The recurrence relationship is $$\small T(h) = 2*\mathcal O(h) + 2*T(h-1)$$, where $$\small h$$ is the height of the tree.

