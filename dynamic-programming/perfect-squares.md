####  Perfect Squares

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



