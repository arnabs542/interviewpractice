Binary Search Trees are some of the most commonly used data structures and can be used to solve almost every data structures problem reasonably efficiently. They offer the ability to efficiently search for a key as well as find the $$\small min$$ and $$\small max$$ elements, look for the successor or predecessor of a search key \(which itself need not be present in the BST\), and enumerate keys in a range in sorted order.

BSTs differ from normal binary trees in that there is an order between keys - the value of each node must be greater than \(or equal to\) any value in its left subtree but less than \(or equal to\) any value in its right subtree.

Key lookup, insertion, and deletion take time proportional to the height of the tree, which can in worst-case be $$\small \mathcal O(n)$$, if insertions and deletions are naively implemented. However, there are implementations of insert and delete which guarantee that he tree has height $$\small \mathcal O(\log{n})$$. Red-black trees are an example of height-balanced BSTs.

In general, avoid putting mutable objects in a BST. Otherwise, when a mutable object that's in a BST is to be updated, removed it first, then update it, then add it back.

Python does not have a built-in BST library. Two options are the sortedcontainers module, which is implemented as a sorted lists of lists, and the bintrees module, which implements sorted sets and sorted dictionaries using balanced BSTs. 

* insert\(e\) inserts new element e into the BST
* discard\(e\) removes e from the BST if present
* min\_item\(\)/max\_item\(\) yields the smallest and largest key-value pair in the BST. 
* min\_key\(\)/max\_key\(\) yields the smallest and largest key in the BST
* pop\_min\(\)/pop\_max\(\) removes and returns the smallest and largest key-value pair in the BST. 

It's important to note that these operations take $$\small \mathcal O(\log{n})$$, since they are backed by the underlying tree. 



