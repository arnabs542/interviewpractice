#### Compute the LCA when Nodes have Parent Pointers

> Given two nodes in a binary tree, design an algorithm that computes their LCA. Assume that each node has a parent pointer.

This can be easily solved using a hash table to store the ancestors of one path, and then traverse the other path and return when we come across an ancestor or reach the root. 

