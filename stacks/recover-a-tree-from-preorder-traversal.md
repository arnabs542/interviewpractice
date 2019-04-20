#### Recover a Tree From Preorder Traversal

> We run a preorder depth first search on the `root` of a binary tree.
>
> At each node in this traversal, we output `D` dashes \(where `D` is the _depth_ of this node\), then we output the value of this node.  _\(If the depth of a node is `D`, the depth of its immediate child is `D+1`.  The depth of the root node is `0`.\)_
>
> If a node has only one child, that child is guaranteed to be the left child.
>
> Given the output `S` of this traversal, recover the tree and return its `root`.
>
> **Example 1:**
>
> ![](/assets/recover_from_preorder_1st.png)
>
> ```
> Input: "1-2--3--4-5--6--7"
> Output: [1,2,5,3,4,6,7]
> ```
>
> **Example 2:**
>
> ![](/assets/recover_preorder_2nd.png)
>
> ```
> Input: "1-2--3---4-5--6---7"
> Output: [1,2,5,3,null,6,null,4,null,7]
> ```
>
> **Example 3:**
>
> ![](/assets/recover_preorder_3rd.png)
>
> ```
> Input: "1-401--349---90--88"
> Output: [1,401,null,349,88,90]
> ```

##### Stack:

```py
def recoverFromPreorder(S):
    stack, i = [], 0
    while i < len(S):
        level, val = 0, ""
        while i < len(S) and S[i] == '-':
            level, i = level + 1, i + 1
        while i < len(S) and S[i] != '-':
            val, i = val + S[i], i + 1
        while len(stack) > level:
            stack.pop()
        node = TreeNode(val)
        if stack and stack[-1].left is None:
            stack[-1].left = node
        elif stack:
            stack[-1].right = node
        stack.append(node)
    return stack[0]
```

We use a construction path to retrace the path of the traversal. In each loop, we get the level of the current node by counting the number of `'-'` and the value of the current node. 

If the size of the stack is bigger than the level of the current node, then we need we need to go up levels by popping the stack until the size is equal. We then try to add the current node as the left subchild of the parent \(if it exist\). If the left child is not None, then the current node is the right child. 

The root is the first element in the stack. 

Running time is $$\small \mathcal O(s)$$, space is $$\small \mathcal O(s)$$ \(for completely skewed tree\).

