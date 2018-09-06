#### Interval Add

> Suppose the time during the day that a person is busy is stored as a set of disjoint time intervals. If an event is added to the person's calendar, the set of busy times may need to be updated. 
>
> In the abstract, we want a way to add an interval to a set of disjoint intervals and represent the new set as a set of disjoint inverals. For example, if the initial set of intervals is \[-4,-1\],\[0,2\],\[3,6\],\[7,9\],\[11,12\],\[14,17\], and the added interval is \[1,8\], the result is \[4,-1\],\[0,9\],\[11,12\],14,17\]. 
>
> Write a program which takes as input an array of disjoint closed intervals with integer endpoints, sorted by increasing order of left endpoint, and an interval to be added, and returns the union of the intervals in the array and the added interval. Your result should be expressed as a union of disjoint intervals sorted by left endpoint.

##### Code:

```py
def add_interval(disjoint_intervals, new_interval):
    res = []
    i = 0

    # Before new interval
    while i < len(disjoint_intervals) and disjoint_intervals[i].right < new_interval.left:
        res.append(disjoint_intervals[i])
        i += 1

    # During new interval
    while i < len(disjoint_intervals) and disjoint_intervals[i].left <= new_interval.right:
        new_interval = Interval(min(disjoint_intervals[i].left, new_interval.left), 
                                max(disjoint_intervals[i].right, new_interval.right))
        i += 1

    return res + [new_interval] + disjoint_intervals[i:]
```

##### Explanation:

Code is pretty self-explanatory. We iterate through the arrays, skipping over all intervals that end strictly before the new interval. We then process all intervals that overlap with the new interval, group the new interval as necessary, before just appending all intervals that begin strictly after the new interval ends. 

