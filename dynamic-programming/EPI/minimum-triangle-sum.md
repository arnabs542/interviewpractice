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
> ```
>
> The minimum path sum from top to bottom is `11` \(i.e., **2** + **3** + **5** + **1** = 11\).

#### Dynamic Programming:

```py
def minimumTotal(triangle):
    if not triangle:
        return 0
    for i in reversed(range(len(triangle)-1)):
        for j in range(len(triangle[i])):
            triangle[i][j] = triangle[i][j] + min(triangle[i+1][j],triangle[i+1][j+1])

    return triangle[0][0]
```

The brute force solution would be to simply perform a DFS search and take the maximum result. The running time would be factorial, since there's 1 choice for the first row, 2 for the second, 3 for the third, and so on. However we can take this path constraint and utilize it to optimize the formula. 

The minimum path must end on one of the nodes in the last row. Each node in the last row \(except the first and last\) has two parents, which in turn have two parents themselves. We can instead build our minimum path sum backwards from the last row and minimize each node by adding it to the smallest of its children. Eventually we reach the very first node, which has to be included in any path. That is our answer. 

Running time is $\small \mathcal O(n)$. We don't use any additional space in this problem because we modify the original array. If we cannot do that, that we use $\small \mathcal O(n)$ space to initialize an extra array that will remember the sum of the partial paths of the row below it. 

