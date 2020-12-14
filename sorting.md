Sorting problems come in two flavors: \(1.\) **use sorting to make the subsequent steps in an algorithm simpler**, and \(2.\) design a **custom sorting routine.** For the former, it's probably find to use a library sort function, possibly with a custom comparator. For the latter, use a data structure like a BST, heap, or array index by values. 

Certain problems become easier to understand, as well as solve, when the input is sorted. The most natural reason to sort is if the inputs have a **natural ordering**, and sorting can be used as a preprocessing step to **speed up searching.**

For **specialized input**, e.g., a very small range of values, or a small numbers of values, it's possible to sort in $$\small \mathcal O(n)$$ time rather than $$\small \mathcal O(n \log {n})$$ time. 

It's often the case that sorting can be implemented in **less space **than required by a brute-force apprach. 



