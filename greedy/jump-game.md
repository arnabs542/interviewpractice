#### Jump Game

> Given an array of non-negative integers, you are initially positioned at the first index of the array.
>
> Each element in the array represents your maximum jump length at that position.
>
> Determine if you are able to reach the last index.
>
> **Example 1:**
>
> ```
> Input: [2,3,1,1,4]
>
> Output: true
>
> Explanation:
> Jump 1 step from index 0 to 1, then 3 steps to the last index.
> ```
>
> **Example 2:**
>
> ```
> Input: [3,2,1,0,4]
>
> Output: false
>
> Explanation:
> You will always arrive at index 3 no matter what. Its maximum jump length is 0, which makes it impossible to reach 
> the last index.
> ```

##### Greedy:

```py
def canJump(nums):
    idx, furthest = 0, 0
    while idx < len(nums) and idx <= furthest:
        furthest = max(furthest, nums[idx] + idx)
        if furthest >= len(nums) - 1:
            return True
        idx += 1
    return False
```

This problem is solved greedily because at each index we try to see the maximum possible jump range. We use a variable `furthest` to remember the maximum coverage we have so far, and as we advance through all the indices within that coverage, we see if any of them can extend `furthest` . 

Running time is $$\small \mathcal O(n)$$. Space usage is constant

