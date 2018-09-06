#### Compute the Union of Intervals

> In this problem we consider sets of intervals with integer endpoints; the intervals may be open or closed at either end. We want to compute the union of the intervals in such sets. 
>
> Design an algorithm that takes as input a set of intervals, and outputs their union expressed as a set of disjoint intervals.

##### Code:

```py
def union_of_intervals(intervals):
    intervals.sort(key=lambda x: (x.left.val, not x.left.is_closed))
    res = []
    for i in intervals:
        # res is empty or strictly none overlap
        if not res or res[-1].right.val < i.left.val:
            res.append(i)
        # endpoints overlap but both endpoints are open
        elif ((res[-1].right.is_closed == i.left.is_closed) and (i.left.is_closed is False)) and (res[-1].right.val == i.left.val):
            res.append(i)
        # i overlaps with last interval in res and one endpoint is closed
        else:
            if i.right.val > res[-1].right.val or (i.right.val == res[-1].right.val and i.right.is_closed):
                res[-1] = Interval(res[-1].left, i.right)
    return res
```

##### Explanation:

There are three situations when we directly append an interval:

1. The result array is empty
2. The current interval begins strictly after the last interval ends
3. The last interval ends exactly where the current interval begins, but both endpoints are open

All other situation requires us to adjust the last added interval in some manner, either by replacing its right endpoint value if the current interval ends later, or changing its closed/open status if the intervals end at the exact same time.

The key stumbling block is to remember to break ties ties by putting the interval with the closed left endpoint first if two endpoints have the same left endpoint value. To see why this is necessary, consider the following input:

`[[119, False, 123, False], [100, True, 108, False], `**`[183, False, 192, True]`**`, [161, True, 164, True], [214, False, 221, False], [184, False, 188, False], [51, True, 52, False], [202, False, 208, False], [216, True, 217, False], [17, True, 24, True], [204, True, 207, False], [57, False, 62, False], [197, False, 204, False], [13, False, 21, True], [80, False, 89, True], [20, True, 27, True], [1, True, 4, True], [91, True, 95, False], [62, False, 71, True], [95, True, 101, True], [225, False, 228, True], `**`[176, False, 183, False]`**`, [184, True, 189, True], `**`[183, True, 192, False]`**`]`

The intervals of importance are bolded. If we sort solely on left endpoints, then we could end up with the following sequence:  **`[176, False, 183, False], [183, False, 192, True], [183, True, 192, False]`**

In this situation, when we encounter the interval beginning at 183 exclusive, we'll append that to our array, based on the elif condition being triggered. This means that our resulting union is incorrect, since the 2nd interval is actually covered by the third interval which has the same left endpoint but closed. If we break ties by prioritizing closed intervals, the three will be correctly merged into one interval.  

