#### Smallest String Starting From Leaf

> Given the `root` of a binary tree, each node has a value from `0` to `25` representing the letters `'a'` to `'z'`: a value of `0` represents `'a'`, a value of `1` represents `'b'`, and so on.
>
> Find the lexicographically smallest string that starts at a leaf of this tree and ends at the root.
>
> _\(As a reminder, any shorter prefix of a string is lexicographically smaller: for example, _`"ab"`_ is lexicographically smaller than _`"aba"`_.  A leaf of a node is a node that has no children.\)_
>
> **Example 1:**
>
> ![](/assets/string_starting_from_leaf_example1.png)
>
> ```
> Input: [0,1,2,3,4,3,4]
> Output: "dba"
> ```
>
> **Example 2:**
>
> ![](/assets/string_start_from_leaf_example2.png)
>
> ```
> Input: [25,1,3,1,3,0,2]
> Output: "adz"
> ```
>
> **Example 3:**
>
> ![](/assets/string_from_leaf_example2.png)
>
> ```
> Input: [2,2,1,null,1,0,null,0]
> Output: "abc"
> ```

##### PostOrder Traversal:

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

We build the string up from every leaf. The current node will append its value to the smaller of the strings from its two children.

The postorder traversal itself takes $$\small \mathcal O(n)$$ time and $$\small \mathcal O(h)$$ space. However, Python strings are immutable, and adding a character to an existing string causes a new string to be allocated. 

PreOrder Traversal:

```py
def smallestFromLeaf(self, root: 'TreeNode') -> 'str':
    self.ans = ""
    
    def dfs(node, cur_string):
        if node:
            cur_string.append(chr(node.val + ord('a')))
            # If leaf check if this is the minimum string
            if not node.left and not node.right:
                if not self.ans:
                    self.ans = "".join(reversed(cur_string))
                else:
                    self.ans = min(self.ans, "".join(reversed(cur_string)))
            
            dfs(node.left, cur_string)
            dfs(node.right, cur_string)
            cur_string.pop()
        
    dfs(root, [])
    return self.ans
```



