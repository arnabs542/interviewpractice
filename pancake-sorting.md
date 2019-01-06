#### Pancake Sorting

> Given an array `A`, we can perform a _pancake flip_: We choose some positive integer `k<= A.length`, then reverse the order of the first **k** elements of `A`.  We want to perform zero or more pancake flips \(doing them one after another in succession\) to sort the array `A`.
>
> Return the k-values corresponding to a sequence of pancake flips that sort `A`.  Any valid answer that sorts the array within `10 * A.length` flips will be judged as correct.
>
> **Example 1:**
>
> ```
> Input: [3,2,4,1]
> Output: [4,2,4,3]
> Explanation: 
> We perform 4 pancake flips, with k values 4, 2, 4, and 3.
> Starting state: A = [3, 2, 4, 1]
> After 1st flip (k=4): A = [1, 4, 2, 3]
> After 2nd flip (k=2): A = [4, 1, 2, 3]
> After 3rd flip (k=4): A = [3, 2, 1, 4]
> After 4th flip (k=3): A = [1, 2, 3, 4], which is sorted.
> ```

##### Sorting:

```py
def pancakeSort(A):
    """
    :type A: List[int]
    :rtype: List[int]
    """
    def flip(A, start, end):
        while start < end:
            A[start], A[end] = A[end], A[start]
            start += 1
            end -= 1

    flips = []
    size = len(A)
    while size > 0:
        max_idx = max(range(size), key=A.__getitem__)
        flips.append(max_idx + 1)
        flip(A, 0, max_idx)
        flips.append(size)
        flip(A, 0, size - 1)
        size -= 1
    return flips
```

 Find the index `max_idx` of the next maximum number `x` in the array. Reverse `i + 1` numbers, so that the largest number will be at `A[0]`. Reverse `i + 1` numbers,  so that `x` will be at `A[x - 1]`. Repeat this process `N` times. Running time is bounded by $$\small \mathcal O(n^{2})$$.

