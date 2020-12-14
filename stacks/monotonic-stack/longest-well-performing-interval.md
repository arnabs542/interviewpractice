#### Longest Well-Performing Interval

> We are given hours, a list of the number of hours worked per day for a given employee.
>
> A day is considered to be a tiring day if and only if the number of hours worked is (strictly) greater than `8`.
>
> A well-performing interval is an interval of days for which the number of tiring days is strictly larger than the number of non-tiring days.
>
> Return the length of the longest well-performing interval.
>
> Example 1:
> ```
> Input: hours = [9,9,6,0,6,6,9]
> Output: 3
> Explanation: The longest well-performing interval is [9,9,6].
> ```

##### Solution

Let's try to break the problem down to more generic terms. If we map all integers greater than `8` to `1` and all integers less to `0`, we're asking what is length of the longest subarray such that the cummulative sum is greater than `k = 0`. 

One way to solve this is to use nest for loops to check all subarrays for validity, then update our answers as we find longer subarrays. However, this takes $\small \mathcal O(n^{2})$ time. However we can actually do this in $\small \mathcal O(n)$ time using a prefix sum array and a monotonic decreasing stack. 

A prefix sum allows us to immediately calculate the sum between two indicies: `prefix_sum[j] - prefix_sum[i] = sum(arr[i], arr[i+1], ..., arr[j])`. We know a subarray `arr[i:j]` is valid if there are more positive values than negative values in it, i.e. `prefix_sum[j] - prefix_sum[i] > k`. 

The monotonic decrease stack allows us to quickly calculate all subarrays that have a potential of doing better than the one we currently have. For example, suppose `(i, j)` is a valid pair if `prefix_sum[j] - prefix[i] > k`. How do we ensure that we find the optimal (longest) pair? 

1. Let us first find all possible indices for `i`. Suppose there are two indices `i1` and `i2` such that `prefix_sum[i1] <= prefix_sum[i2]`. In this case, there is no point in considering `i2`, because if there exists some index `j` such that `(i1, j)` and `(i2, j)` are bot h valid pairs, then `i1` comes before `i2` and is therefore longer. In other words, in order for `i2` to be a possible starting point, it needs to be lower than the previous possible starting point, otherwise it cannot possibly give us a longer subarray.

2. Having found all starting points, we now look for ending points. Consider any `j1` and `j2` such that `i < j1 < j2` and `prefix_sum[j2] - prefix[i] > k`. In this situation, there is no need to consider `j1` because `j2` gives us a strictly longer subarray. What this tells us is that we should iterate `j` backwards and if we find a valid `(i, j)` combination using a starting location in our stack, we don't need to keep it any longer.

```py
def longestWPI(hours: "List[int]") -> int:
    stack = []
    
    prefix = [0] * (1 + len(hours))
    for i in range(1, len(hours) + 1):
        prefix[i] += prefix[i-1] + (1 if hours[i-1] > 8 else -1)
    
    for i, e in enumerate(prefix):
        if not stack or prefix[stack[-1]] > e:
            stack.append(i) # Push index not value
    
    res = 0 
    for i in range(len(hours), -1, -1):
        while stack and prefix[i] > prefix[stack[-1]]:
            res = max(res, i - stack[-1])
            stack.pop()
    
    return res
```

The above solution is a general approach to solving questions that ask for the longest subarray with a sum greater than some number. 

Similar questions:

* [Shortest Subarray with Sum at Least K](queues/monotonic-queues/shortest-subarray-with-sum-at-least-K.md)
* [Maximum Width Ramp](stacks/monotonic-stack/maximum-width-ramp.md)