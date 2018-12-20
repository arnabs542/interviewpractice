#### Maximum Sum Circular Subarray

> Given a **circular arrayC** of integers represented by `A`, find the maximum possible sum of a non-empty subarray of **C**.
>
> Here, a _circular array_ means the end of the array connects to the beginning of the array.  \(Formally, `C[i] = A[i]` when `0 <= i < A.length`, and `C[i+A.length] = C[i]` when `i >= 0`.\)
>
> Also, a subarray may only include each element of the fixed buffer `A` at most once.  \(Formally, for a subarray `C[i], C[i+1], ..., C[j]`, there does not exist `i <= k1, k2 <= j` with `k1 % A.length = k2 % A.length`.\)
>
> **Example 1:**
>
> ```
> Input: [1,-2,3,-2]
> Output: 3
>
> Explanation: Subarray [3] has maximum sum 3
> ```
>
> **Example 2:**
>
> ```
> Input: [5,-3,5]
> Output: 10
>
> Explanation:Subarray [5,5] has maximum sum 5 + 5 = 10
> ```
>
> **Example 3:**
>
> ```
> Input: [3,-1,2,-1]
> Output: 4
>
> Explanation:
> Subarray [2,-1,3] has maximum sum 2 + (-1) + 3 = 4
> ```
>
> **Example 4:**
>
> ```
> Input: [3,-2,2,-3]
> Output: 3
>
> Explanation: 
> Subarray [3] and [3,-2,2] both have maximum sum 3
> ```
>
> **Example 5:**
>
> ```
> Input: [-2,-3,-1]
> Output: -1
>
> Explanation:
> Subarray [-1] has maximum sum -1
> ```

##### Kadane's Algorithm:

```py
def maxSubarraySumCircular(A):

    total, maxSum, curMax, minSum, curMin = 0, -float('inf'), 0, float('inf'), 0
    for a in A:
        curMax = max(curMax + a, a)
        maxSum = max(maxSum, curMax)
        curMin = min(curMin + a, a)
        minSum = min(minSum, curMin)
        total += a
    return max(maxSum, total - minSum) if maxSum > 0 else maxSum
```

The above algorithm is really a modified version of Kadane's algorithm on case analysis:

1. The maximum subarray is not circular. In that case, we could find the answer with Kadane's algorithm. 
2. The maximum subarray is circular. What that means is that there is a minimum subarray sum somewhere in the middle of the array that we can remove from the sum of all elements in the array. 

Refer to the picture below:

![](/assets/max_sum_circular.png)

Therefore, we can solve this using one pass. As we go through, we remember the maximum continuous subarray sum. We also remember the minimum continuous subarray sum. At the end of the loop, `maxSum, total - minSum` would  respectively give us the maximum subarray for a noncircular subarray and the maximum subarray for a circular subarray. 

##### Edge Cases:

We do need to check if maxSum is positive. In the event all elements in the array is negative, we would return `total - minSum` since that would equal 0. However, if 0 does not actually appear in the array, the answer is wrong; `maxSum` would contain the largest negative number, which would be the correct answer.

