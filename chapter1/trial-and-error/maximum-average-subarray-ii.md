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

Again, this 

