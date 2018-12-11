#### Delete Columns to Make Sorted II

> We are given an array `A` of `N` lowercase letter strings, all of the same length.
>
> Now, we may choose any set of deletion indices, and for each string, we delete all the characters in those indices.
>
> For example, if we have an array `A = ["abcdef","uvwxyz"]` and deletion indices `{0, 2, 3}`, then the final array after deletions is `["bef","vyz"]`.
>
> Suppose we chose a set of deletion indices `D` such that after deletions, the final array has its elements in **lexicographic** order \(`A[0] <= A[1] <= A[2] ... <= A[A.length - 1]`\).
>
> Return the minimum possible value of `D.length`.
>
> **Example 1:**
>
> ```
> Input: ["ca","bb","ac"]
> Output: 1
> Explanation: 
> After deleting the first column, A = ["a", "b", "c"].
> Now A is in lexicographic order (ie. A[0] <= A[1] <= A[2]).
> We require at least 1 deletion since initially A was not in lexicographic order, so the answer is 1.
> ```
>
> **Example 2:**
>
> ```
> Input: ["xc","yb","za"]
> Output: 0
> Explanation: 
> A is already in lexicographic order, so we don't need to delete anything.
> Note that the rows of A are not necessarily in lexicographic order:
> ie. it is NOT necessarily true that (A[0][0] <= A[0][1] <= ...)
> ```
>
> **Example 3:**
>
> ```
> Input: ["zyx","wvu","tsr"]
> Output: 3
> Explanation: 
> We have to delete every column.
> ```

##### Edge Cases:

Same letters:

```py
def minDeletionSize(A):

    min_delete = 0
    n = len(A)

    def is_ordered(i):
        column = [x[i] for x in A]
        for i in range(1, len(column)):
            if column[i] < column[i-1]:
                return False
        return True

    for i in range(len(A[0])):
        if is_ordered(i):
            return min_delete
        min_delete += 1

    return min_delete
```

My initial approach was to just examine each column at a time. If a column was out of order, then delete it and increment our delete counter. However, in a case like `A = ["xga","xfb","yfa"]`, this algorithm is in correct. The first column is in order, but the second column is not. However, once we delete the second column, we end up with `A = ["xa","xb","ya"]`, which is in order. However, we look at the third column by itself, we see `["a", "b", "a"]`, which is not in order, and we increment the delete counter again.

The problem with this greedy approach is it doesn't take into context whether the previous columns take care of later columns. In this particular case, we should stop after deleting the second column.

