# Search for a Range

```py
def searchRange(nums, target):

    range_idx = [-1, -1]

    # First idx
    lo, hi = 0, len(nums) - 1
    while lo <= hi:
        mid = (lo + hi) // 2
        if nums[mid] == target:
            range_idx[0] = mid
            hi = mid - 1
        elif nums[mid] > target:
            hi = mid - 1
        else:
            lo = mid + 1

    # Last idx
    lo, hi = 0, len(nums) - 1
    while lo <= hi:
        mid = (lo + hi) // 2
        if nums[mid] == target:
            range_idx[1] = mid
            lo = mid + 1
        elif nums[mid] > target:
            hi = mid - 1
        else:
            lo = mid + 1

    return range_idx
```

In terms of templates, the above solution more closely resembles template 1. We start with inclusive bounds, and we terminate when all candidates have been tested. In the first search, we just go as far left as possible, remembering each time we see an occurrence of the target. In the second search we go as far right as possible, also recording down each time we see the target.

Overall the run time should be $$\small \mathcal O(\log(n))$$, but the constant setting of variables actually slows it down a bit. A better solution would be to use a variable to keep track of the range bounds, then set them once:

```py
def searchRange(nums, target):

    range_idx = [-1, -1]

    if not nums:
        return range_idx

    # First idx
    lo, hi = 0, len(nums) - 1
    left = right = -1
    while lo <= hi:
        mid = (lo + hi) // 2
        if nums[mid] == target:
            left = mid
            hi = mid - 1
        elif nums[mid] > target:
            hi = mid - 1
        else:
            lo = mid + 1

    if nums[left] != target:
        return range_idx
    range_idx[0] = left

    # Last idx
    lo, hi = 0, len(nums) - 1
    while lo <= hi:
        mid = (lo + hi) // 2
        if nums[mid] == target:
            right = mid
            lo = mid + 1
        elif nums[mid] > target:
            hi = mid - 1
        else:
            lo = mid + 1

    range_idx[1] = right
    return range_idx
```

The first time I implemented the above code I forgot to check for null inputs, which was dealt with in the original solution in the while loops. In the event of a null input, `hi` starts off lower than `lo`, so we never even enter the loop. In the second solution, because we directly access `range_idx` to set the values, we need to watch out for null inputs.

The algorithm below follows template 2 more closely:

```py
def searchRange(nums, target):

    range_idx = [-1, -1]

    if not nums:
        return range_idx

    # First idx
    lo, hi = 0, len(nums) - 1
    while lo < hi:
        mid = (lo + hi) // 2
        if nums[mid] < target:
            lo = mid + 1
        else:
            hi = mid

    if nums[lo] != target:
        return range_idx
    range_idx[0] = lo

    # Last idx
    hi = len(nums) - 1
    while lo < hi:
        mid = (lo + hi) // 2 + 1
        if nums[mid] > target:
            hi = mid - 1
        else:
            lo = mid

    range_idx[1] = hi
    return range_idx
```

Again, the main idea is the same: use two binary searches to find the boundaries. The difference is how the termination condition and boundary updates are set up.

We start with an inclusive search space of `[0, len(nums) - 1]`. From the value at `nums[mid]`, there are three possibilities:

1. `nums[mid] < target`    The range must begin on the **right** of mid. Update `lo = mid + 1` for next iteration
2. `nums[mid] == target`    The range must begin on the **left** or **at** mid. Update `hi = mid` for next iteration
3. `nums[mid] > target`    The range must begin on the **left** of mid. Update `hi = mid - 1`for next iteration.

Since we move the hi boundary for two conditions, we just combine it into a single case:

```py
if nums[mid] >= target:
    hi = mid
```

We're able to combine the two because our termination condition allows for one candidate to be left in the search space. Otherwise, we would run into an infinite loop. Eventually our search space will shrink to just 2 elements. If the target is the first element, then we will update `hi = mid`, which will equal `lo`, since it's the floor of the average between the two. If the target is the second element, then we will update `lo = mid + 1`, which will make it equal to hi and we will terminate.

For the right of the range, we can use a similar idea. We start with an inclusive search space of `[lo, len(nums) - 1]`. There is no point in starting at 0, because we've already found the left most bound. From the value at `nums[mid]`, there are three possibilities:

1. `nums[mid] > target`    The range must begin on the **right** of mid. Update `hi = mid - 1` for next iteration
2. `nums[mid] == target`    The range must begin on the **right** or **at** mid. Update lo`= mid` for next iteration
3. `nums[mid] < target`    The range must begin on the **left** of mid. Update lo`= mid + 1`for next iteration.

Again, we can merge condition 2 and 3 into:

```py
if nums[mid] <= target:
    i = mid
```

However, the terminate condition on longer works this time. Consider the following case:

```
[5 7], target = 5
```

Now `nums[mid] = 5`, then according to rule 2, we set `i = mid`. This practically does nothing because i is already equal to mid. As a result, the search range is not moved at all.

To solve this, we calculate `mid` slightly differently: `mid = (lo + hi) // 2 + 1`

This is to "bias" mid towards hi, so that it will always be greater than lo. Again, binary searches run into infinite loops when no new information is provided regarding the search bounds between iterations. By biasing mid, we know that `lo != mid`, and by setting `lo = mid`, we're still ensuring the interval shrinks.

