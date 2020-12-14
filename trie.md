A `Trie` is a special form of a `Nary tree`. Typically, a trie is used to `store strings`. Each Trie node represents `a string` \(`a prefix`\). Each node might have several children nodes while the paths to different children nodes represent different characters. And the strings the child nodes represent will be the `origin string` represented by the node itself plus `the character on the path`.

Here is an example of a trie:

![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/02/07/screen-shot-2018-01-31-at-163403.png)

In the example, the value we mark in each node is the string the node represents. For instance, we start from the root node and choose the second path 'b', then choose the first child 'a', and choose child 'd', finally we arrived at the node "bad". The value of the node is exactly formed by the letters in the path from the root to the node sequentially.

It is worth noting that the `root` node is associated with the `empty string`.

One important property of Trie is that all the descendants of a node have a common prefix of the string associated with that node. That's why Trie is also called `prefix tree`.

Let's look at the example again. For example, the strings represented by nodes in the subtree rooted at node "b" have a common prefix "b". And vice versa. The strings which have the common prefix "b" are all in the subtree rooted at node "b" while the strings with different prefixes will come to different branches.

