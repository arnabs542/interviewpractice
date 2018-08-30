#### Merge Two Sorted Arrays

> Suppose you are given two sorted arrays of integers. If one array has enough empty entries at its end, it can be used to store the combined entries of the two arrays in sorted order. For example, consider $$\small <3,13,17,\_ ,\_,\_,\_,\_>$$ and $$\small <3,7,11,19>$$, where $$\small \_$$ denotes an empty entry. Then the combined sorted entries can be stored in the first array as $$\small <3,3,7,11,13,17,19,\_>$$. Write a program which takes as input two sorted arrays of integers, and updates the first to the combined entries of the two arrays in sorted order. Assume the first array has enough empty entries at its end to hold the result.

##### Code:

```py
def merge_two_sorted_arrays(A, m, B, n):
    idx = m + n - 1
    a, b = m - 1, n - 1
    while a > - 1 and b > - 1:
        if A[a] < B[b]:
            A[idx] = B[b]
            b -= 1
        else:
            A[idx] = A[a]
            a -= 1
        idx -= 1
    while b > - 1:
        A[idx] = B[b]
        idx, b = idx - 1, b - 1
    return
```

##### Explanation:

Since we are told that the first array has enough space for the final solution, we simply work backwards to figure out the position each element should go in. The time complexity is again $$\small \mathcal O(m+n)$$. Note for these simultaneous pointer problems, it's entire possible that we traverse on array completely before traversing the other array completely. 

Note that our final while loop deals only with the second array. If the second array has already by processed, then the rest of elements in array A were already in their correct places to begin with, and no extra work needs to be done. 

