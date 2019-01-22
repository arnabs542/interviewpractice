#### Rotate Array

> Given an array, rotate the array to the right by _k _steps_, _where _k_ is non-negative.
>
> **Example 1:**
>
> ```
> Input: [1,2,3,4,5,6,7] and k = 3
>
> Output: [5,6,7,1,2,3,4]
>
> Explanation:
> rotate 1 steps to the right: [7,1,2,3,4,5,6]
> rotate 2 steps to the right: [6,7,1,2,3,4,5]
> rotate 3 steps to the right: [5,6,7,1,2,3,4]
> ```
>
> **Example 2:**
>
> ```
> Input: [-1,-100,3,99] and k = 2
>
> Output: [3,99,-1,-100]
>
> Explanation: 
> rotate 1 steps to the right: [99,-1,-100,3]
> rotate 2 steps to the right: [3,99,-1,-100]
> ```

The simplest way to solve this problem would be to construct a new array and move the elements one at a time to their new positions. However, the problem asks the operation be done in place. We can continuously move the last element to the front and shift all $$\small n - 1$$ elements back one. This takes $$\small \mathcal O(n^{2})$$ time, and will time out.

##### Reverse and Rotate:

```py
def rotate(nums, k):

    def reverse(start, end, A):
        while start < end:
            A[start], A[end] = A[end], A[start]
            start += 1
            end -= 1

    k %= len(nums)

    reverse(0, len(nums) - 1, nums)
    reverse(0, k - 1, nums)
    reverse(k, len(nums) - 1, nums)
```

This is a classic way to rotate an array to the right by $$\small k$$ elements. The steps are as follows:

1. Reverse the entire array

2. Reverse the first k elements

3. Reverse the rest of the elements

Suppose the input was $$\small [1,2,3,4,5,6,7], k = 3$$. After reversing the array, we have $$\small [7,6,5,4,3,2,1]$$. Reversing the first 3 elements, we get $$\small [5,6,7,4,3,2,1]$$. Reversing the rest of the elements results in $$\small [5,6,7,1,2,3,4]$$.

Running time is $$\small O(n)$$.

