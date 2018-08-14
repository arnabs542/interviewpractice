# Find K Closest Elements

```py
def findClosestElements(arr, k, x):

    max_heap = []
    for e in arr:        
        if len(max_heap) < k or abs(e - x) < abs(-max_heap[0][0]):
            heapq.heappush(max_heap, [-abs(e - x), e])
        if len(max_heap) == k + 1:
            heapq.heappop(max_heap)        
    res = list(sorted([x[1] for x in max_heap]))
    return res
```

The immediate solution I thought of was using a heap. Overall time complexity should be $$\small \mathcal O(n \log(k))$$. To insert/delete an element from a heap of size $$\small k$$ is $$\small \mathcal O(\log(k))$$, and since there are $$\small n$$ elements in the array, the initial building of the heap should take the majority of the time. The sorting will be $$\small \mathcal O(k\log(k))$$ time, which should be smaller than the $$\small \mathcal O(n \log(k))$$ time, assuming $$\small k << n$$.

Using binary search, we can reduce the time complexity to $$\small \mathcal O(\log(n) + k)$$. We use binary search to find the element, or the closest element slightly greater if the element is not in the array. Afterwards, we build the correct range.

```py
def findClosestElements(self, arr, k, x):

    lo, hi = 0, len(arr) - 1
    # Get as close to x as possible
    while lo < hi:
        mid = (lo + hi) // 2
        if arr[mid] == x:
            hi = mid
            break
        elif arr[mid] < x:
            lo = mid + 1
        else:
            hi = mid

    lo = hi - 1

    # find ranges
    while k > 0:
        if lo < 0 or (hi < len(arr) and abs(x - arr[lo]) > abs(x - arr[hi])):
            hi += 1
        else:
            lo -= 1
        k -= 1

    return arr[lo + 1 : hi]
```

Notice how we're doing our binary search here. We use an inclusive bound with a 1-candidate-left termination condition. We set `lo = mid + 1` if the number is strictly too small, but we set `hi = mid` if the number is too big. After termination, we set `lo = hi - 1`. We only increase `hi` if `lo` is already less than 0, or if `nums[hi]` is strictly closer to `x`. We want to start our array from `lo + 1` because we move `lo` after we judge `nums[lo]` to be closer. Similarly, we move `hi` after we judge `nums[hi]` to be closer, so we don't want to include the element `hi` last points to.

We can adjust the algorithm to find the closest item that's smaller than the target as well:

```py
def findClosestElements(arr, k, x):

    lo, hi = 0, len(arr) - 1

    # Get as close to x as possible
    while lo < hi:
        mid = (lo + hi) // 2 + 1
        if arr[mid] == x:
            lo = mid
            break
        elif arr[mid] < x:
            lo = mid
        else:
            hi = mid - 1

    hi = lo + 1

    # find ranges
    while k > 0:
        if hi >= len(arr) or (lo > -1 and abs(x - arr[lo]) <= abs(x - arr[hi])):
            lo -= 1
        else:
            hi += 1
        k -= 1

    return arr[lo + 1 : hi]
```

Below is an algorithm using a library implemented binary search:

```py
def findClosestElements(arr, k, x):

    left = right = bisect.bisect_left(arr, x)
    while right - left < k:
        if left == 0: return arr[:k]
        if right == len(arr): return arr[-k:]
        if x - arr[left - 1] <= arr[right] - x: left -= 1
        else: right += 1
    return arr[left:right]
```

The early termination logic is as follow:

1. If `left == 0` and we're still in the loop, that means x is smaller than the smallest element in the arr. Return the k smallest elements.
2. If `right == len(arr)` and we're still in the loop, that means x is larger than the largest element in the array. Return k larggest elements.

We return `arr[left:right]`because we're using `left` to point to the last test element, instead of next to be tested element. On the other hand, `right`points to the current right element that needs to be tested.

