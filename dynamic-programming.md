#### Dynamic Programming

Consider using DP whenever you have to **make choices** to arrive at the solution. Specifically, DP is applicable when you can construct a solution to the given instance from solutions to sub-instances of smaller problems of the same kind.

In addition to optimization problems, DP is also **applicable** to **counting and decision problems** - any problems where you can express a solution recursively in terms of the same computation on smaller instances. 

Although conceptually DP involves recursion, often for efficiency the cache is **build "bottom-up"**, i.e. iteratively.

When DP is implemented recursively the cache is typically a dynamic data structure such as a hash table or a BST; when it is implemented iteratively the cache is usually a one- or multi-dimensional array.

To save space, **cache space** may be **recycled **once it is known that a set of entries will not be looked up again. 

Sometimes, **recursion may out-perform a bottom-up DP **solution, e.g., when the solution is found early or subproblems can be **pruned** through bounding. 

A commong **mistake** in solving DP problems is trying to think of the recursive case by splitting the problems into **two equal halves**, _a la_ quicksort, i.e. solve the subproblems for subarrays A\[0, n/2\] and A\[n/2+1, n\] and combine the results. However, in most cases, these two subproblems are not sufficient to solve the original problem. 

DP is based on **combining solutions** to subproblems to **yield a solution** to the original problem. However, for some problems DP will not work. For example, if you need to compute the longest path from City 1 to City 2 without repeating an intermediate city, and this longest path passes through City 3, then the subpaths from City 1 to City 3 and City 3 to City 2 may not be individually longest paths without repeated cities.



