#### Number of Longest Increasing Subsequence

> Given an unsorted array of integers, find the number of longest increasing subsequence.
>
> **Example 1:**  
>
>
> ```
> Input: [1,3,5,4,7]
> Output: 2
>
> Explanation: The two longest increasing subsequence are [1, 3, 4, 7] and [1, 3, 5, 7].
> ```
>
> **Example 2:**  
>
>
> ```
> Input: [2,2,2,2,2]
> Output: 5
>
> Explanation: The length of longest continuous increasing subsequence is 1, 
> and there are 5 subsequences' length is 1, so output 5.
> ```

##### Dynamic Programming:

```js
def findNumberOfLIS(nums):

    if not nums:
        return 0
    length = [1] * len(nums)
    count = [1] * len(nums)
    for i in range(len(nums)):
        for j in range(i):
            if nums[j] < nums[i]:
                if length[i] <= length[j]:
                    length[i] = length[j] + 1
                    count[i] = count[j]
                elif length[i] == length[j] + 1:
                    count[i] += count[j]
    maximum = max(length)
    return sum(count[k] for k in range(len(nums)) if length[k] == maximum)
```

This is a slight modification of the parent problem. Instead of simply remembering the LIS, we now need to figure out how many there are. 

To do so, we use another table count. While `length[i]` continues to records the LIS that includes `nums[i]`, `count[i]` records how many sequences there are. 

For every `i < j` with `A[i] < A[j]`, we might append `A[j]` to a longest subsequence ending at `A[i]`. It means that we have demonstrated `count[i]` subsequences of length `length[i] + 1`.

Now, if those sequences are longer than `length[j]`, then we know we have `count[i]` sequences of this length. If these sequences are equal in length to `length[j]`, then we know that there are now `count[i]` additional sequences to be counted of that length \(ie. `count[j] += count[i]`\).

Running time is still $$\small \mathcal O(n^{2})$$.

