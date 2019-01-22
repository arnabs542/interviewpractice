A subarray `A[i], A[i+1], ..., A[j]` of `A` is said to be _turbulent_ if and only if:

* For `i <= k < j`, `A[k] > A[k+1]` when `k` is odd, and `A[k] < A[k+1]` when `k` is even;
* **OR**, for `i <= k < j`, `A[k] > A[k+1]` when `k` is even, and `A[k] < A[k+1]` when `k` is odd.

That is, the subarray is turbulent if the comparison sign flips between each adjacent pair of elements in the subarray. Return the **length** of a maximum size turbulent subarray of A.

**Example 1:**

```
Input: [9,4,2,10,7,8,8,1,9]
Output: 5
Explanation: (A[1] > A[2] < A[3] > A[4] < A[5])
```

**Example 2:**

```
Input: [4,8,12,16]
Output: 2
```

**Example 3:**

```
Input: [100]
Output: 1
```

##### Sliding Window:

```py
def maxTurbulenceSize(A):
    """
    :type A: List[int]
    :rtype: int
    """
    if not A:
        return 0
    
    count = max_count = 1
    prev_sign = None
    for i in range(1, len(A)):
        cur_sign = (A[i] - A[i-1]) - (A[i-1] - A[i])
        if cur_sign != prev_sign and cur_sign != 0:
            count += 1
            max_count = max(max_count, count)                
        else:
            count = 2        
        prev_sign = cur_sign

    return max_count
```



