#### Find the Minimum Weight Path in a Triangle

> Given a triangle, find the minimum path sum from top to bottom. Each step you may move to adjacent numbers on the row below.
>
> For example, given the following triangle
>
> ```
> [
>      [2],
>     [3,4],
>    [6,5,7],
>   [4,1,8,3]
> ]
> ```
>
> The minimum path sum from top to bottom is `11` \(i.e., **2** + **3** + **5** + **1** = 11\).

##### Dynamic Programming:

```py
def minimum_path_weight(triangle):
    if len(triangle) == 0 or len(triangle[0]) == 0:
        return 0

    for i in reversed(range(len(triangle)-1)):
        for j in range(len(triangle[i])):
            triangle[i][j] += min(triangle[i+1][j], triangle[i+1][j+1])

    return triangle[0][0]
```

The main idea here is to work backward. Since we have to go from the top to bottom, that means we have to end at one of the cells in the last row. To get there, we must go through one of the cells through the second to last row, and etc. Furthermore, since we're only allowed to move to adjacent entries, we can figure out the smallest possible weight for each cell for its path to the last row. For example, suppose there are $$\small n$$ rows. The cell at `triangle[n-2][0]` can only move to either `triangle[n-1][0]` or `triangle[n-1][1]`. Thus, if we start at `triangle[n-2][0]`, the smallest path we can have to the bottom is:

`triangle[n-2][0] += min(triangle[n-1][0], triangle[n-1][1])`

We simply work backwards until we get to the first cell.

The running time is bounded by $$\small \mathcal O(n)$$, where $$\small n$$ is the number of elements in the triangle. We use the given triangle as our dp array, so we don't use any additional space. If we're not allowed to modify the triangle, we can accomplish this with two arrays of length $$\small m$$, where $$\small m$$ is the number of elements in the second to last row:

```py
def minimum_path_weight(triangle):
    if not triangle or not triangle[0]:
        return 0

    cur_row = triangle[-1]
    for i in reversed(range(len(triangle) - 1)):
        for j in range(len(triangle[i])):
            cur_row[j] = triangle[i][j] + min(cur_row[j], cur_row[j+1])

    return cur_row[0]
```

##### Notes:

Remember to check for empty inputs whenever dealing with array like structures.

