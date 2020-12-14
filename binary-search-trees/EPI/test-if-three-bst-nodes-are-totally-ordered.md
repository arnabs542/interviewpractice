#### Test if Three BST Nodes are Totally Ordered

> Write a program which takes two nodes in a BST and a third node, the "middle" node, and determines if one of the two nodes is a proper ancestor and the other a proper descendant of the middle. \(A proper ancestor of a node is an ancestor that is not equal to the node; a proper descendant is defined similarly\). Assume all keys are unique. Nodes do not have pointers to their parents.

##### Code:

```py
def pair_includes_ancestor_and_descendant_of_m(possible_anc_or_desc_0,
                                               possible_anc_or_desc_1, middle):

    search_0, search_1 = possible_anc_or_desc_0, possible_anc_or_desc_1

    while (search_0 is not possible_anc_or_desc_1 and search_0 is not middle
           and search_1 is not possible_anc_or_desc_0
           and search_1 is not middle and (search_0 or search_1)):
        if search_0:
            search_0 = (search_0.left
                        if search_0.data > middle.data else search_0.right)
        if search_1:
            search_1 = (search_1.left
                        if search_1.data > middle.data else search_1.right)

    if ((search_0 is not middle and search_1 is not middle)
            or search_0 is possible_anc_or_desc_1
            or search_1 is possible_anc_or_desc_0):

        return False

    def search_target(source, target):
        while source and source is not target:
            source = source.left if source.data > target.data else source.right
        return source is target

    return search_target(middle, possible_anc_or_desc_1
                         if search_0 is middle else possible_anc_or_desc_0)
```

##### Explanation:

The brute force solution would be pick one of the inputs as ancestor, and try to find middle from it. If that doesn't work, try with the other input. The problem is suppose we pick a node that is a direct child of middle - we'll end up traversing the entire height of the tree before trying with the other node. Granted, in the event that the middle node doesn't exist or both given nodes are ancestors, we'll need to traverse the entire tree anyways, but we should be able to optimize the situation that we actually have a valid solution.

The idea here is that we treat both nodes as possible ancestors, and try to find middle. While at least one node is valid, and we don't end up finding the other node, we simply continue down the tree until we find the middle or reach one of our terminating conditions. If we find one node from the other, that means that the middle node is not between the two nodes - they could both be ancestors, or maybe the middle node doesn't exist.

Suppose that one of the nodes is a parent, we now try to find the other node from the middle node. Overall runtime is still $\small \mathcal O(h)$ worst-case, but in the event that a valid solution exists, we'll often be able to finish a little faster. When the middle node does have an ancestor and descendant in the pair, the time complexity is $\small \mathcal O(d)$, where $\small d$ is the difference between the depths of the acnestor and descendant. The reason is that the interleaved search will stop when the ancestor reaches the middle node, i.e., after $\small \mathcal O(d)$ iterations. The search from the middle node to the descendant takes $\small \mathcal O(d)$ steps to succeed as well. 

