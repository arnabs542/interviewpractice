#### Minimum Number of Days for N Bouquets

> You are given an array of roses. `roses[i]` means rose `i` will bloom on day `roses[i]`. You are also given an integer `k`, which is the minimum number of adjacent bloom roses required for a bouquet, and an integer `n`, which is the number of bouquets we need. Return the earliest day that we can get `n` bouquets of roses.
>
> Example:
> ```
> Input: roses = [1, 2, 4, 9, 3, 4, 1], k = 2, n = 2
> Output: 4
> Explanation:
> day 1: [b, n, n, n, n, n, b]
> The first and the last rose bloom.
> 
> day 2: [b, b, n, n, n, n, b]
> The second rose blooms. Here the first two bloom roses make a bouquet.
> 
> day 3: [b, b, n, n, b, n, b]
>
>day 4: [b, b, b, n, b, b, b]
> Here the last three bloom roses make a bouquet, meeting the required n = 2 bouquets of bloom roses. So return day 4.
>```

#### Solution

My first reaction to was try to use a disjoint-set data structure to solve this. While that could work, we can actually use binary search to solve the problem. The key realization is that we actually know the bounds we're searching in - the earliest possible day is the lowest day in `roses[i]`, and the latest possible day is the maximum in the array (assuming this problem is guaranteed to have a solution).

The trick is setting up the "trial part of this problem". The solution is actually closely related to the problem [Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/).

Let's look at the example given above: `roses = [1, 2, 4, 9, 3, 4, 1], k = 2, n = 2`. We will group `roses` into bundles of size `2`, and we will set the bundle value to the maximum day in that bundle:
```
roses =      [1,2,4,9,3,4,1]
bundle_day = [2,4,9,9,4,4]
```

Each entry in `bundle_day` the earliest day we can pick the bouquet of size `k` starting at index `i`. For example, to pick the bouquet at `roses[:2]`, we have to wait until day 2, because the second rose doesn't bloom until then. To pick the bouquet at `roses[1:3]`, we have to wait until day `4`, and so on. This will be what we use for the trial portion. 

Once we have the above array, the problem become very straightforward. Give a day, we iterate through the array. If we can pick the bouquet at `bundle_day[i]`, we increment `i` by `k`. Otherwise, incremenet by 1.

Full code is below:

```py
# calculate the earliest we can pick the bundle at index i
def bundle_roses(roses, k, bundle):
    queue = collections.deque()
    for i in range(roses):
        if i >= k:  # remove out of range days
            while queue and queue[0] <= i-k:
                queue.pop()
        # remove smaller days in current bundle
        while queue and roses[queue[-1]] < roses[i]:
            queue.pop()
        queue.append(i)
        if i >= k-1:
            bundle[i-k+1] = roses[queue[0]]

def can_fulfill(bundle, days, n, k):
    i = 0
    while i < len(bundle):
        if bundle[i] <= days:
            n -= 1
            i += k
        else:
            i += 1
    return n == 0

def min_days_bloom(roses, k, n):
    # initial setup
    k = max(len(roses), k)
    bundle_max_days = [0] * (len(roses)-k+1)
    bundle_roses(roses, k, bundle_max_days)

    # begin binary search
    start, end = min(roses), max(roses)
    while start <= end:
        mid = (end + start) >> 1
        if can_fulfill(bundle_max_days, mid, n, k):
            end = mid - 1
        else:
            start = mid + 1
    return end + 1
```

Running time is $\small \mathcal O(n * \log{r_{max} - r_{min}})$. The `bundle_roses` array is length $\small \mathcal O(n - k)$, and the search range depends on the maximum and minimum values in the `roses` array.