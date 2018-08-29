#### Trial and Error

There is a subset of problems that can best be described as "trial and error". These problem often use binary search in their solution, but it is not a direct application and usually requires some pre-processing work to be done.

Normally we would refrain from using the naive trial and error algorithms for solving problems since it generally leads to bad time performance. However, there are situations where this naive algorithm may outperform other more sophisticated solutions, and LeetCode does have a few such problems \(listed at the end of this post -- ironically most of them are "hard" problems\).

The basic idea for the trial and error algorithm is actually very simple and summarized below:

`Step 1`: Construct a candidate solution.  
`Step 2`: Verify if it meets our requirements.  
`Step 3`: If it does, accept the solution; else discard it and repeat from `Step 1`.

However, to make this algorithm work efficiently, the following two conditions need to be true:

**Condition 1**: We have an efficient verification algorithm in `Step 2`;  
**Condition 2**: The search space formed by all candidate solutions is small or we have efficient ways to traverse \(or search\) this space if it is large.

The first condition ensures that each verification operation can be done quickly while the second condition limits the total number of such operations that need to be done. The two combined will guarantee that we have an efficient trial and error algorithm \(which also means if any of them cannot be satisfied, you should probably not even consider this algorithm\).

The binary search part of the solution usually comes in satisfying condition 2 - if we are able to establish a search space, and each iteration we are able to discard half of the space, then we generally have a good a good way to process the search space, since this will usually take us $$\small \mathcal O (\log{n})$$ iterations. We usually want to see if we can limit **condition 1** to around $$\small \mathcal O(n)$$ time to give us a $$\small \mathcal O(n\log(n))$$ time complexity.



Other problems of this type:

	• 786. K-th Smallest Prime Fraction

	• 774 Minimize Max Distance to Gas Station

	• 719. Find K-th Smallest Pair Distance

	• 668. Kth Smallest Number in Multiplication Table

	• 644. Maximum Average Subarray II

	• 378. Kth Smallest Element in a Sorted Matrix

