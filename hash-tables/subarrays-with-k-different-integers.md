#### Subarrays with K Different Integers

> Given an array `A` of positive integers, call a \(contiguous, not necessarily distinct\) subarray of `A`_good_ if the number of different integers in that subarray is exactly `K`.
>
> \(For example, `[1,2,3,1,2]` has `3` different integers: `1`, `2`, and `3`.\)
>
> Return the number of good subarrays of `A`.
>
> **Example 1:**
>
> ```
> Input: A = [1,2,1,2,3], K = 2
> Output: 7
> Explanation: 
> Subarrays formed with exactly 2 different integers: [1,2], [2,1], [1,2], [2,3], [1,2,1], [2,1,2], [1,2,1,2].
> ```
>
> **Example 2:**
>
> ```
> Input: A = [1,2,1,3,4], K = 3
> Output: 3
> Explanation: Subarrays formed with exactly 3 different integers: [1,2,1,3], [2,1,3], [1,3,4].
> ```

##### Brute Force:

```py
def subarraysWithKDistinct(A: 'List[int]', K: 'int') -> 'int':
    count = 0
    for i in range(len(A)):
        elements = collections.defaultdict(int)
        for j in range(i, len(A)):
            elements[A[j]] += 1
            if len(elements) == K:
                count += 1
            elif len(elements) > K:
                break
    return count
```

The brute force solution is just to run a nested for loop going over all subarrays, then check how many elements each subarray has. Increment the counter if the number of distinct elements is $$\small K$$.

Runtime is $$\small \mathcal O(n^{2})$$. Space usage is $$\small \mathcal O(n)$$.

##### Sliding Window:        

```py
def subarraysWithAtMostK(A: 'List[int]', K: 'int') -> 'int':
    window = collections.defaultdict(int)
    res = i = j = 0
    while j < len(A):
        if window[A[j]] == 0:
            K -= 1
        window[A[j]] += 1
        while K < 0:
            window[A[i]] -= 1
            if window[A[i]] == 0:
                K += 1
            i += 1
        res += j - i + 1
        j += 1
    return res
    
def subarraysWithKDistinct(A: 'List[int]', K: 'int') -> 'int':
    return subarraysWithAtMostK(A, K) - subarraysWithAtMostK(A, K-1)
```



