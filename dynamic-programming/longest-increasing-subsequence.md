Longest Increasing Subsequence



Binary Search:

```py
def lengthOfLIS(nums):
    
    chain = []
    
    def search(x):
        lo, hi = 0, len(chain) - 1
        while lo < hi:
            mid = (lo + hi) >> 1
            if chain[mid] < x:
                lo = mid + 1
            else:
                hi = mid
        return lo
    
    for e in nums:
        if not chain or e > chain[-1]:
            chain.append(e)
        else:
            idx = search(e)
            chain[idx] = e
    
    return len(chain)
```



