#### Maximum Width Ramp

> Given an array `A` of integers, a _ramp_ is a tuple `(i, j)` for which `i < j` and `A[i] <= A[j]`.  The width of such a ramp is `j - i`.
>
> Find the maximum width of a ramp in `A`.  If one doesn't exist, return 0.
>
> **Example 1:**
>
> ```
> Input: [6,0,8,2,1,5]
> Output: 4
>
> Explanation: 
> The maximum width ramp is achieved at (i, j) = (1, 5): A[1] = 0 and A[5] = 5.
> ```
>
> **Example 2:**
>
> ```
> Input: [9,8,1,0,1,9,4,0,4,1]
> Output: 7
>
> Explanation: 
> The maximum width ramp is achieved at (i, j) = (2, 9): A[2] = 1 and A[9] = 1.
> ```

##### Sorting:

```py
def maxWidthRamp(A):
    
    min_ind, max_ramp, inds = float('inf'), 0, collections.defaultdict(list)
    
    for i,e in enumerate(A):
        inds[e].append(i)
    
    for i,e in enumerate(sorted(A)):
        max_ramp = max(max_ramp, inds[e][-1] - min_ind)
        min_ind = min(min_ind, inds[e][0])
    
    return max_ramp
```



