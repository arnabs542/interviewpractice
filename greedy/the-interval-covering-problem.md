#### The Interval Covering Problem

> Consider a foreman responsible for a number of tasks on the factory floor. Each task starts at a fixed time and ends at a fixed time. The foreman wants to visit the floor to check on the tasks. Your job is to help him minimize the number of visits he makes. In each visit, he can check on all the tasks taking place at the time of the visit. A visit takes place at a fixed time, and he can only check on tasks taking place at exactly that time. For example, if there are tasks at times `[0,3], [2,6], [3,4], [6,9]`, then visit times `0, 2, 3, 6` cover all tasks. A smaller set of visit times that also cover all tasks is `3, 6`. In the abstract, you are to solve the following problem.
>
> You are given a set of closed intervals. Design an efficient algorithm for finding a minimum sized set of numbers that covers all the intervals.

##### Sorting:

```py
def find_minimum_visits(intervals):

    intervals.sort(key=lambda x: x.right)
    last_visit_time, num_visits = float('-inf'), 0
    for interval in intervals:
        if interval.left > last_visit_time:
            num_visits += 1
            last_visit_time = interval.right

    return num_visits
```

Note that we can restrict out attention to numbers which are endpoints without losing optimality. We could thus simply return the left and right endpoints of every interval, but we already showed in the example that does not always produce an optimal answer. Similarly, greedily picking an endpoint which covers the most intervals may also lead to suboptimum results, e.g., consider the six intervals `[1,2], [2,3], [3,4], [2,3], [3,4], [4,5]`. The point `3` appears in four intervals, more than any other point. However, if we choose `3`, we do not cover `[1,2]` and `[4,5]`, so we need two additional points to cover all six intervals. If we pick `2` and `4`, each individually covers just three intervals, but combined they cover all six.

The key is to fix on extreme cases, in particular, consider the interval that ends first. To cover it, we must pick a number that appears in it. Furthermore, we should pick its right endpoint, since any other intervals covered by a number in the interval will continue to be covered if we pick the right endpoint \(otherwise the interval we began with cannot be the interval that ends first\). Once we choose that endpoint, we can remove all other covered intervals, and continue with the remaining set. 

Thus, we have our algorithm: sort all the intervals, comparing on right endpoints. Select the first interval's right endpoint. Iterate through the intervals, looking for the first one not covered by this right endpoint. As soon as such an interval is found, select its right endpoint and continue the iteration. 

Running time is dominated by the sort: $$\small \mathcal O(n \log{n})$$.

