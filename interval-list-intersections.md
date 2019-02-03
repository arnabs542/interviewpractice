#### Interval List Intersections

> Given two lists of **closed** intervals, each list of intervals is pairwise disjoint and in sorted order.
>
> Return the intersection of these two interval lists.
>
> _\(Formally, a closed interval `[a, b]` \(with `a <= b`\) denotes the set of real numbers `x` with `a <= x <= b`.  The intersection of two closed intervals is a set of real numbers that is either empty, or can be represented as a closed interval.  For example, the intersection of \[1, 3\] and \[2, 4\] is \[2, 3\].\)_
>
> **Example 1:**
>
> ![](https://assets.leetcode.com/uploads/2019/01/30/interval1.png)
>
> ```
> Input: 
> A = [[0,2],[5,10],[13,23],[24,25]], B = [[1,5],[8,12],[15,24],[25,26]]
> Output: [[1,2],[5,5],[8,10],[15,23],[24,24],[25,25]]
> Reminder: The inputs and the desired output are lists of Interval objects, and not arrays or lists.
> ```

##### Sort + Two Pointer:

```py
def intervalIntersection(A: 'List[Interval]', B: 'List[Interval]') -> 'List[Interval]':
    intersects = []
    i = j = 0
    
    while i < len(A) and j < len(B):
        # Check for intersection
        start = max(A[i].start, B[j].start)
        end = min(A[i].end, B[j].end)
        if start <= end:
            intersects.append(Interval(start, end))
            
        # Remove Interval with smallest endpoint
        if A[i].end < B[j].end:
            i += 1
        else:
            j += 1
    
    return intersects
```

First thing we need to do is sort the intervals by starting points. This is already done for us in the problem. 

The two pointer logic is as follows:

* If `A[0]` has the smallest endpoint, it can only intersect `B[0]`. After, we can discard `A[0]` since it cannot intersect anything else.
* Similarly, if `B[0]` has the smallest endpoint, it can only intersect `A[0]`, and we can discard `B[0]` after since it cannot intersect anything else.
* We use two pointers, `i` and `j`, to virtually manage "discarding" `A[0]` or `B[0]` repeatedly.

Running time is $$\small \mathcal O(n + m)$$, where $$\small n,m$$ are the lengths of the two arrays.

