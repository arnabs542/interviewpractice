#### Minimum Triangle Sum

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
>
> ```
>
> The minimum path sum from top to bottom is `11` \(i.e., **2** + **3** + **5** + **1** = 11\).

```py
def minimumTotal(triangle):
    if not triangle:
        return 0
    for i in reversed(range(len(triangle)-1)):
        for j in range(len(triangle[i])):
            triangle[i][j] = triangle[i][j] + min(triangle[i+1][j],triangle[i+1][j+1])
    
    return triangle[0][0]
```



