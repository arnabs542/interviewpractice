#### Jump Game II

> Given an array of non-negative integers, you are initially positioned at the first index of the array.
>
> Each element in the array represents your maximum jump length at that position.
>
> Your goal is to reach the last index in the minimum number of jumps.
>
> **Example:**
>
> ```
> Input: [2,3,1,1,4]
>
> Output: 2
>
> Explanation:
> The minimum number of jumps to reach the last index is 2.
> Jump 1 step from index 0 to 1, then 3 steps to the last index.
> ```

##### Greedy:

```py
def jump(nums):

    jumps = jump_range = idx = cur_max = 0

    while idx < len(nums) and idx <= jump_range:
        cur_max = max(idx + nums[idx], cur_max)
        if idx == len(nums) - 1:
            return jumps
        if cur_max == len(nums) - 1:
            return jumps + 1
        if idx == jump_range:
            jumps += 1
            jump_range = cur_max
        idx += 1
```

Suppose the range of the current jump from some index $$\small i$$ is `[i, jump_range]`. Within that interval, `cur_max` is the farthest point that all points within `[i, jump_range]` can reach. Once `idx` reaches `jump_range`, we trigger another jump, and set the new `jump_range` as `cur_max`.

Running time is $$\small \mathcal O(n)$$. We can use another variable `jump_idx` to keep track of which point gives the furthest `cur_max` in each interval, and store those to get a jump sequence of minimum jumps.

