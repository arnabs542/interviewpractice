#### Squares of Sorted Array

> Given an array of integers `A` sorted in non-decreasing order, return an array of the squares of each number, also in sorted non-decreasing order.
>
> **Example 1:**
>
> ```
> Input: [-4,-1,0,3,10]
> Output: [0,1,9,16,100]
> ```
>
> **Example 2:**
>
> ```
> Input: [-7,-3,2,3,11]
> Output: [4,9,9,49,121]
> ```

We could simply go through the array, square every number, then re-sort the array. This approach takes $$\small O(n \log{n})$$ time. However, this disregards the pre-sorted nature of the input array.

##### Two-pointer:

```py
def sortedSquares(A):
    """
    :type A: List[int]
    :rtype: List[int]
    """
    res = []
    start, end = 0, len(A) - 1
    while start <= end:
        start_square, end_square = A[start]**2, A[end]**2
        if start_square > end_square:
            res.append(start_square)
            start += 1
        else:
            res.append(end_square)
            end -= 1
    return res[::-1]
```

The key to the above solution is realizing that the largest element of the square array must come from either end of the array. Once we realize that property, we simply have two pointers at both ends of the array, and move them depending on which element gives the larger square.

Runtime is $$\small \mathcal O(n)$$.

