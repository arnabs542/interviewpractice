#### Non-overlapping Intervals

> Given a collection of intervals, find the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping.
>
> **Note:**
>
> 1. You may assume the interval's end point is always bigger than its start point.
> 2. Intervals like \[1,2\] and \[2,3\] have borders "touching" but they don't overlap each other.
>
> **Example 1:**
>
> ```
> Input: [ [1,2], [2,3], [3,4], [1,3] ]
>
> Output: 1
>
> Explanation: 
> [1,3] can be removed and the rest of intervals are non-overlapping.
> ```
>
> **Example 2:**
>
> ```
> Input: [ [1,2], [1,2], [1,2] ]
>
> Output: 2
>
> Explanation:
> You need to remove two [1,2] to make the rest of intervals non-overlapping.
> ```
>
> **Example 3:**
>
> ```
> Input: [ [1,2], [2,3] ]
>
> Output: 0
>
> Explanation:
> You don't need to remove any of the intervals since they're already non-overlapping.
> ```

##### Sorting:

```py
def eraseOverlapIntervals(intervals):
    
    intervals.sort(key=lambda x: x.end)
    
    cur_interval = None
    count = 0
    for i in range(len(intervals)):
        if cur_interval is None or intervals[i].start >= cur_interval.end:
            count += 1
            cur_interval = intervals[i]
    
    return len(intervals) - count
```

This is another sort by endpoint problem. If we sort by starting point, an input like $$\small [[1,6],[2,3],[3,4],[4,5],[5,6]]$$ would result in us deleting 4 intervals when simply delete the first one is enough. If we try to select the shortest interval when faced with a conflict, an input like $$\small [[1,6],[5,8],[6,7]]$$ would result in 2 deletions instead of 1. 

A short explanation of why a greedy solution based on sorting endpoints works is as follows:

When we select an interval $$\small i$$ to keep, we may need to remove many intervals after it. But this is the correct choice, because all of the intervals overlap with the finishing time of $$\small i$$, which means they all overlap with one another. Thus, for every interval in the optimal solution, there is an interval in the greedy solution.  

Runtime time is still dominated by the initial sort.

