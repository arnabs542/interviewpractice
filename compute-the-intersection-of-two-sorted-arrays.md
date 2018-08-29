#### Compute the Intersection of Two Sorted Arrays

> Write a program which takes as input two sorted arrays, and returns a new array containing elements that are present in both of the input arrays. The input arrays may have duplicate entries, but the returned array should be free of duplicates. For example, the input is $$\small <2,3,3,5,5,6,7,7,8,12>$$ and $$\small <5,5,6,8,8,9,10,10>$$, your output should be $$\small <5,6,8>$$.

##### Code \(Hash set\):

```py
def intersect_two_sorted_arrays(A, B):
    return list(sorted(set(A) & set(B)))
```

##### Code \(Iteration\):

```py
def intersect_two_sorted_arrays(A, B):
    intersection = []
    i = j = 0
    while i < len(A) and j < len(B):
        if A[i] < B[j]:
            i += 1
        elif A[i] > B[j]:
            j += 1
        else:
            if i == 0 or A[i] != A[i-1]:
                intersection.append(A[i])
            i, j = i + 1, j + 1
    return intersection
```

##### Explanation:

The fact that the arrays are already sorted for us makes the problem much easier. We could use extra space by putting both arrays into sets, but we could do this in $$\small \mathcal O(m+n)$$ time. Note that the time complexity is not bounded by $$\small \mathcal O(min(m,n))$$, sincewe could advance one pointer all the way till the last element of one array, which could be greater than all other elements in the other array, resulting in us needing to traverse both arrays completely.

