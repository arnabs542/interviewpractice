#### Maximum Average Subarray II

> Given an array with positive and negative numbers, find the `maximum average subarray` which length should be greater or equal to given length `k`.
>
> ### Example
>
> Given nums = `[1, 12, -5, -6, 50, 3]`, k = `3`
>
> Return `15.667` // \(-6 + 50 + 3\) / 3 = 15.667
>
> ### Notice
>
> It's guaranteed that the size of the array is greater or equal to _k_.

```py
def maxAverage(self, nums, k):

    def can_find(nums, avg, k):
        acc, count = 0, 0
        for i in range(len(nums)):
            for j in range(i+k, len(nums) + 1):
                if sum(nums[i:j])/(j - i) >= avg:
                    return True
        return False


    # write your code here
    lo, hi = min(nums), max(nums)
    while not math.isclose(lo, hi):
        mid = (lo + hi) / 2  # bias mid towards high
        if lo == 5e-324 or hi == 5e-324 or mid == 5e-324:
            return 0
        if can_find(nums, mid, k):
            lo = mid
        else:
            hi = mid
    return lo
```

Since we already know this is a trial-and-error problem, let's first establish the search space. The maximum possible average is equal to the max element in the array, while the smallest possible average is equal to the min element in the array. 

It's important to note the peculiarity with the number 5e-324, which causes some issues with comparisons to 0. I think this has something to do with floating representation in python but I'm not sure. 

The verification portion timed out, since the above implementation is at worst $$\small \mathcal O(n^{2})$$ time. 

