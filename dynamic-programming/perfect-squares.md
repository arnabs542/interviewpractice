#### Perfect Squares

> Given a positive integer n, find the least number of perfect square numbers \(for example, `1, 4, 9, 16, ...`\) which sum to n.
>
> **Example 1:**
>
> ```
> Input: n = 12
> Output: 3 
>
> Explanation: 
> 12 = 4 + 4 + 4.
> ```
>
> **Example 2:**
>
> ```
> Input: n = 13
> Output: 2
>
> Explanation: 
> 13 = 4 + 9.
> ```

##### Code:

```py
def numSquares(n):

    if n <= 2:
        return n

    squares = []
    i = 0
    while i**2 <= n:
        squares.append(i**2)
        i += 1

    to_check = {n}
    cnt = 0
    while to_check:
        temp = set()
        cnt += 1
        for x in to_check:
            for sq in squares:
                if x == sq:
                    return cnt
                elif x < sq:
                    break
                temp.add(x - sq)
        to_check = temp
    return cnt
```

The overall idea here is to constantly reduce all possible candidates until we hit a base case of a square. 

We start by generating all perfect squares less than equal to our target. For example, for n = 12, squares = \[0,1,4,9\]. We being with to\_check = {12}. The code will execute as follows:

```
cnt    squares    to_check (start)    to_check (end)
1     [0,1,4,9]         {12}             {11,8,3}
2        ""          {11,8,3}          {10,7,2,4}
3        ""         {10,7,2,4}        {9,5,1,6,3,1,0}               
```



