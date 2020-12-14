#### Minimum Area Rectangle

> Given a set of points in the xy-plane, determine the minimum area of a rectangle formed from these points, with sides parallel to the x and y axes.
>
> If there isn't any rectangle, return 0.
>
> **Example 1:**
>
> ```
> Input: [[1,1],[1,3],[3,1],[3,3],[2,2]]
> Output: 4
> ```
>
> **Example 2:**
>
> ```
> Input: [[1,1],[1,3],[3,1],[3,3],[4,1],[4,3]]
> Output: 2
> ```
>
> **Note**:
>
> 1. `1 <= points.length <= 500`
> 2. `0 <= points[i][0] <= 40000`
> 3. `0 <= points[i][1] <= 40000`
> 4. All points are distinct.

##### Code:

```py
def minAreaRect(self, points):

    seen = set()
    min_area = float('inf')

    for x1,y1 in points:
        for x2,y2 in seen:
            if (x1,y2) in seen and (x2,y1) in seen:
                min_area = min(min_area, abs(x1-x2) * abs(y1-y2))
        seen.add((x1,y1))

    return 0 if min_area == float('inf') else min_area
```

The above code runs in $\small \mathcal O(n^{2})$ time. We basically run a nested for loop picking out pairs of points that we guess to be diagonals of the minimum rectangle. If the other two points of the rectangle exist, then we calculate and update the area as necessary. Otherwise, we continue picking points.

