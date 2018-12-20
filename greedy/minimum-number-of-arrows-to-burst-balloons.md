#### Minimum Number of Arrows to Burst Balloons

> There are a number of spherical balloons spread in two-dimensional space. For each balloon, provided input is the start and end coordinates of the horizontal diameter. Since it's horizontal, y-coordinates don't matter and hence the x-coordinates of start and end of the diameter suffice. Start is always smaller than end. There will be at most 104 balloons.
>
> An arrow can be shot up exactly vertically from different points along the x-axis. A balloon with xstart and xend bursts by an arrow shot at x if xstart ≤ x ≤ xend. There is no limit to the number of arrows that can be shot. An arrow once shot keeps travelling up infinitely. The problem is to find the minimum number of arrows that must be shot to burst all balloons.
>
> **Example:**
>
> ```
> Input:[[10,16], [2,8], [1,6], [7,12]]
>
>
> Output:2
>
>
> Explanation:
>
> One way is to shoot one arrow for example at x = 6 (bursting the balloons [2,8] and [1,6]) 
> and another arrow at x = 11 (bursting the other two balloons).
> ```

##### Sorting by start:

```py
def findMinArrowShots(points):

    points.sort(key=lambda x: x[0])

    ballons = []
    for p in points:
        if not ballons or p[0] > ballons[-1][1]:
            ballons.append(p)
        else:
            ballons[-1] = [min(p[0], ballons[-1][1]), min(ballons[-1][1], p[1])]

    return len(ballons)
```

The main idea here is to pack as many balloons as possible into one arrow. For example, suppose the balloons were at $$\small [[7,10], [1,5], [3,6], [2,4], [1,4]]$$. After sorting it becomes $$\small [[1,4], [1,5], [2,4], [3,6], [7,10]]$$. The first balloon will be left as $$\small [1,4]$$, since an arrow at any point in that range will pop it. The second balloon overlaps will the the first, we change the existing hit range to a tigher bound. The valid hit range is still $$\small [1,4]$$, because that is the tighter range. The third balloon overlaps with the previous 2, and the hit range is now $$\small [2,4]$$. The fourth balloon change the hit range to $$\small [3,4]$$. The fifth balloon requires a new range, so we need 2 arrows to pop all balloons. Runtime is bounded by $$\small O(n \log{n})$$ due to sort. Space is bounded by $$\small \mathcal O(n)$$.

##### Sorting by end:

```py
def findMinArrowShots(points):

    points.sort(key=lambda x: x[1])

    last_thrown = float('-inf')
    thrown = 0

    for p in points:
        if p[0] > last_thrown:
            thrown += 1
            last_thrown = p[1]

    return thrown
```

We sort by endpoint here because we're going to shoot the arrow as far right as possible. For each balloon, the arrow must fall on/between `balloon[0]` and `balloon[1]`. If we fire the arrow at `balloon[1]`, that gives us the most wiggle room, since it will hit and pop any other balloon that starts before that point. Runtime is the same, but space is now constant. 

