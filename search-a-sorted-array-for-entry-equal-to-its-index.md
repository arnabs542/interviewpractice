# Search a Sorted Array for Entry Equal to Its Index

```py
def search_entry_equal_to_its_index(A):
    lo, hi = 0, len(A) - 1
    while lo <= hi:
        mid = lo + (hi - lo) // 2
        if A[mid] == mid:
            return mid
        elif A[mid] > mid:
            hi = mid - 1
        else:
            lo = mid + 1
    return -1
```



