#### Maximum Sum of Two Non-Overlapping Subarrays

> Given an array `A` of non-negative integers, return the maximum sum of elements in two non-overlapping \(contiguous\) subarrays, which have lengths `L` and `M`.  \(For clarification, the `L`-length subarray could occur before or after the `M`-length subarray.\)
>
> Formally, return the largest `V` for which `V = (A[i] + A[i+1] + ... + A[i+L-1]) + (A[j] + A[j+1] + ... + A[j+M-1])` and either:
>
> * `0 <= i < i + L - 1 < j < j + M - 1 < A.length`, **or**
> * `0 <= j < j + M - 1 < i < i + L - 1 < A.length`.
>
> **Example 1:**
>
> ```
> Input: A = [0,6,5,2,2,5,1,9,4], L = 1, M = 2
> Output: 20
> 
> Explanation: One choice of subarrays is [9] with length 1, and [6,5] with length 2.
> ```
>
> **Example 2:**
>
> ```
> Input: A = [3,8,1,3,2,1,8,9,0], L = 3, M = 2
> Output: 29
>
> Explanation: One choice of subarrays is [3,8,1] with length 3, and [8,9] with length 2.
> ```
>
> **Example 3:**
> 
> ```
> Input: A = [2,1,5,6,0,9,5,0,3,8], L = 4, M = 3
> Output: 31
>
> Explanation: One choice of subarrays is [5,6,0,9] with length 4, and [3,8] with length 3.
>```

##### Brute Force:

```py
def maxSumTwoNoOverlap(A: List[int], L: int, M: int) -> int:
    max_sum = 0

    if L < M:
        L, M = M, L


    def helper(A, length):
        if len(A) < length:
            return 0
        best_sum = 0
        cur_sum = sum(A[:length-1])
        for i in range(length-1, len(A)):
            cur_sum += A[i]
            if i >= length:
                cur_sum -= A[i-length]
            best_sum = max(best_sum, cur_sum)
        return best_sum

    L_sum = sum(A[:L-1])
    for i in range(L-1, len(A)):
        L_sum += A[i]
        if i >= L:
            L_sum -= A[i-L]
        M_sum = max(helper(A[:i-L+1], M), helper(A[i+1:], M))
        max_sum = max(L_sum + M_sum,  max_sum)

    return max_sum
```

The brute force solution is essentially the exact same solution as the brute force approach for the parent problem. We iterate the array, splitting it in half at each index, then we try to find the maximum sum in both halves.

Running time is $\small \mathcal O(n^{2})$.

##### Four-pass solution:

```py
def maxTwoNoOverlap(A, L, M):
    n = len(A)
    left_l_max_sum, right_m_max_sum = [0] * n, [0] * n
    
    l_sum = 0
    for i in range(n):
        l_sum += A[i]
        if i >= L:
            l_sum -= A[i - L]
        left_l_max_sum[i] = max((0 if i == 0 else left_l_max_sum[i-1]), l_sum)
        
    m_sum = 0
    for i in range(n-1, -1, -1):
        m_sum += A[i]
        if n - i > M:
            m_sum -= A[i + M]
        right_m_max_sum[i] = max((0 if i == n-1 else right_m_max_sum[i+1]), m_sum)
    
    max_sum = 0
    for i in range(n-1):
        max_sum = max(max_sum, left_l_max_sum[i] + right_m_max_sum[i+1])
    
    #print(left_l_max_sum, right_m_max_sum)
    
    return max_sum

def maxSumTwoNoOverlap(A: List[int], L: int, M: int) -> int:
    return max(self.maxTwoNoOverlap(A, L, M), self.maxTwoNoOverlap(A, M, L))
```

The optimized solution is also extremely similar to the one for the parent problem. Instead of maximum profit, we just track the maximum sum of $\small N$ elements.

Left-to-right, track the maximum sum of `L` elements in `left_l_max_sum`. Right-to-left, track the maximum sum of `M` elements in `right`.Then, find the split point where `left[i] + right[i]` gives us the maximum sum. A key point to note is that we need to do this twice for `(L, M)` and `(M, L)`.

**Note:** If we come across problems that may require splitting the array in 2 halves, consider using this approach. 

