#### Max Consecutive Ones III

> Given an array `A` of 0s and 1s, we may change up to `K` values from 0 to 1. Return the length of the longest \(contiguous\) subarray that contains only 1s.
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

##### Brute Force \(TLE\):

```py
def longestOnes(A: "List[int]", K: "int") -> "int":
    if not A:
        return 0
    # Find all components first
    max_ones = 0
    for i in range(len(A)):
        cur_ones, cur_flips = 0, K
        for j in range(i, len(A)):
            if A[j] == 1:
                cur_ones += 1
            else:
                if cur_flips > 0:
                    cur_ones += 1
                    cur_flips -= 1
                else:
                    break
        max_ones = max(max_ones, cur_ones)
    max_ones = max(max_ones, cur_ones)
    return max_ones
```

The brute force approach is simply to use a nested for loop to try to start at each element and see how far we can go. This takes $$\small \mathcal O(n^{2})$$ time quickly times out.

##### Sliding Window:

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

The key to the above approach is to reframe the question. In essence, we're simply asking what's the longest subarray we can have that contains at most $$\small K$$ 0s. This is perfect for using a sliding window. We grow the array until we have more than the allowed 0s, then we shrink the array until we're within the valid limit again. This requires only $$\small \mathcal O(n)$$ time, since each element is processed at most once.

