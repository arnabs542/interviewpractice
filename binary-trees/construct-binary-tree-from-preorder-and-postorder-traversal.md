#### Construct Binary Tree from Preorder and Postorder Traversal

> Return any binary tree that matches the given preorder and postorder traversals.
>
> Values in the traversals `pre` and `post` are distinct positive integers.
>
> **Example 1:**
>
> ```
> Input: 
> pre = [1,2,4,5,3,6,7], post = [4,5,2,6,7,3,1]
> Output: [1,2,3,4,5,6,7]
> ```
>
> **Note:**
>
> * `1 <= pre.length == post.length <= 30`
> * `pre[]` and `post[]` are both permutations of `1, 2, ..., pre.length`.
> * It is guaranteed an answer exists. If there exists multiple answers, you can return any of them.

This problem is interesting because a binary tree cannot be uniquely constructed from its preorder and postorder traversal, simply because we don't know which nodes belong on the left and which nodes on the right. For example, consider the trees below:

```
    1       1 
   /       /
  2       2
 /         \ 
3           3 
```

Both trees have the same preorder traversal: \[1,2,3\] and postorder traversal: \[3,2,1\]. This may seem as an issue at first, but since we're asked to return any valid tree, we can use this to our advantage. 

##### Code \(Unoptimized\):

```py
def constructFromPrePost(pre, post):
    if not pre:
        return None
    root = TreeNode(pre[0])
    if len(pre) == 1:
        return root

    L = post.index(pre[1]) + 1
    root.left = self.constructFromPrePost(pre[1:L+1], post[:L])
    root.right = self.constructFromPrePost(pre[L+1:], post[L:])
    return root
```

##### Explanation:

A preorder traversal can be summarized as such: `(root node) (preorder of left branch) (preorder of right branch)`. A postorder traversal: `(postorder of left branch) (postorder of right branch) (root node)`. Let us assume the left branch has $\small L$ nodes. We also know that the root of the left branch occurs at `pre[1]`. Since the values are all unique, that same node can be uniquely identified in the postorder traversal as well. Therefore, we look for the index of `pre[1]` in the postorder traversal, because its index marks a potential split between the left and right subtrees. Remember that the current root is located at `pre[0]`. Since the preorder and postorder traversals of the left branch must have the same number of nodes, and since the postorder traversal starts with the left branch, the location of $\small L$ in the postorder array is from the beginning of the array, which makes it the length of the branch. We then simply build the rest of the tree recursively. 

One big drawback of the above method is that we keep passing in copies of the arrays in our recursive calls. This can be sped up.

##### Code \(Optimized\): 

```py
def constructFromPrePost(pre, post):

    stack = [TreeNode(pre[0])]
    j = 0

    for v in pre[1:]:
        node = TreeNode(v)
        while stack[-1].val == post[j]:
            stack.pop()
            j += 1
        if not stack[-1].left:
            stack[-1].left = node
        else:
            stack[-1].right = node
        stack.append(node)

    return stack[0]
```

Explanation:

The code above solves the problem in $\small O(n)$ time and space. The key idea is we use a stack to generate TreeNodes in a preorder fashion, then pop them out in a postorder fashion. 

To understand, look again at how postorder traversal works: `(postorder of left branch) (postorder of right branch) (root node)`. If we encounter a node \(all nodes values are distinct\) in a postorder traversal, that means we have finished traversing all of its children.

We loop through the preorder array, constructing nodes as we encounter them. If a node happens to the same value as the current postorder node we're waiting on, that means the node and all its children have been processed, and can be popped. Otherwise, we try to stack left if no left child, else right. 

