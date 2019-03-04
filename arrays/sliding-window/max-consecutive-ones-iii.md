#### Max Consecutive Ones III

> Given an array `A` of 0s and 1s, we may change up to `K` values from 0 to 1. Return the length of the longest \(contiguous\) subarray that contains only 1s. 
>
> **Example 1:**
>
> ```
> Input: A = [1,1,1,0,0,0,1,1,1,1,0], K = 2
> Output: 6
> Explanation: [1,1,1,0,0,1,1,1,1,1,1]
> We flip the 5th and 10th 0s to 1s.
> ```
>
> **Example 2:**
>
> ```
> Input: A = [0,0,1,1,0,0,1,1,1,0,1,1,0,0,0,1,1,1,1], K = 3
> Output: 10
> Explanation: [0,0,1,1,1,1,1,1,1,1,1,1,0,0,0,1,1,1,1]
> We flip the 4th, 5th, and 9th 0s to 1s.
> ```

Brute Force \(TLE\):



Sliding Window:

```py
def longestOnes(A: "List[int]", K: "int") -> "int":
        if not A:
            return 0
        start = end = 0
        cur_count = max_count = 0
        cur_zeros = 0
        while end < len(A):
            if A[end] == 1:
                cur_count += 1
            else:
                cur_zeros += 1
                if cur_zeros <= K:
                    cur_count += 1
                else:
                    while cur_zeros > K:
                        if A[start] == 1:
                            cur_count -= 1
                        else:
                            cur_zeros -= 1
                        start += 1
            max_count = max(cur_count, max_count)
            end += 1
        return max_count
```



