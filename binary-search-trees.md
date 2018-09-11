Binary Search Trees are some of the most commonly used data structures and can be used to solve almost every data structures problem reasonably efficiently. They offer the ability to efficiently search for a key as well as find the $$\small min$$ and $$\small max$$ elements, look for the successor or predecessor of a search key \(which itself need not be present in the BST\), and enumerate keys in a range in sorted order. 

BSTs differ from normal binary trees in that there is an order between keys - the value of each node must be greater than \(or equal to\) any value in its left subtree but less than \(or equal to\) any value in its right subtree. 

Key lookup, insertion, and deletion take time proportional to the height of the tree, which can in worst-case be $$\small \mathcal O(n)$$, if insertions and deletions are naively implemented. However, there are implementations of insert and delete which guarantee that he tree has height $$\small \mathcal O(\log{n})$$. Red-black trees are an example of height-balanced BSTs.

In general, avoid putting mutable objects in a BST. Otherwise, when a mutable object that's in a BST is to be updated, removed it first, then update it, then add it back. 

