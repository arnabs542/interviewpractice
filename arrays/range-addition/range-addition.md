#### Range Addition

> Assume you have an array of length `n` initialized with all `0`'s and are given `k` update operations.
>
> Each operation is represented as a triplet: `[startIndex, endIndex, inc]` which increments each element of subarray `A[startIndex ... endIndex]` (`startIndex` and `endIndex` inclusive) with `inc`.
>
> Return the modified array after all k operations were executed.
>
> Example:
> ```
> Given:
>
>    length = 5,
>    updates = [
>        [1,  3,  2],
>        [2,  4,  3],
>        [0,  2, -2]
>    ]
>
> Output:
>
>    [-2, 0, 3, 5, 3]
>
> Explanation:
>
> Initial state:
> [ 0, 0, 0, 0, 0 ]
>
> After applying operation [1, 3, 2]:
> [ 0, 2, 2, 2, 0 ]
>
> After applying operation [2, 4, 3]:
> [ 0, 2, 5, 5, 3 ]
>
> After applying operation [0, 2, -2]:
> [-2, 0, 3, 5, 3 ]
> ```

#### Solution

The brute force solution is trivial - for each triplet we get, simply iterate through the entire range and add the increment to all values. However, the time complexity is $\small \mathcal O(mn)$ worst case, where $\small m$ is the total range. This occures when all $\small n$ triplets span the entire range.

The key to this problem is realzing that we only care about the borders. Given a triplet, starting from `startIndex`, every cell will be increased by `inc` until the cell after the `endIndex`. We can use a prefix sum to achieve this - at the `startIndex`, we add `inc`. At the `endIndex`, we add negative `inc`. As we update the prefix sum array, every index between `startIndex` and `endIndex` will include `inc`, while every index after `endIndex` will be cancelled out by the negative `inc` at `endIndex + 1 `.

```py
def range_addition(n, ranges):
    res = [0] * n
    for s, e, i in ranges:
        res[s] += i
        if e + 1 < n:
            res[e + 1] = -i
    
    for i in range(1, n):
        res[i] += res[i - 1]
    
    return res
```

Runtime and space are both $\small \mathcal O(n)$