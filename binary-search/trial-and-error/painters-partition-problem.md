#### Painter's Partition Problem

> You have to paint `N` boards of length `{A0, A1, A2, A3 … AN-1}`. There are `K` painters available and you are also given how much time a painter takes to paint `1` unit of board. You have to get this job done as soon as possible under the constraints that any painter will only paint contiguous sections of board.
>
> * 2 painters cannot share a board to paint. That is to say, a board cannot be painted partially by one painter, and partially by another.
> * A painter will only paint contiguous boards. Which means a configuration where painter 1 paints board 1 and 3 but not 2 is invalid.
>
> Return the ans % 10000003
>```
> Input :
> K : Number of painters
> T : Time taken by painter to paint 1 unit of board
> L : A List which will represent length of each board
>
> Output:
>     return minimum time to paint all boards % 10000003
>```
> Example
>```
> Input : 
>  K : 2
>  T : 5
>  L : [1, 10]
>
> Output : 50
>```

##### Solution 

This is a very good problem for trial and error, because we know we can finish in `sum(C)` time. Therefore, we have a defined search space in which the final answer must lie. This allows us to use binary search to find that answer.

```py
def paint(A, B, C):

    def can_paint(time, C):
        painters = A - 1
        cur_work = 0
        for c in C:
            if c * B > time:
                return False
            if cur_work + c * B <= time:
                cur_work += c * B
            else:
                if not painters:
                    return False
                painters -= 1
                cur_work = c * B
        return True
            
    
    min_time, max_time = 0, sum(C) * B
    while min_time < max_time:
        mid = (min_time + max_time) >> 1
        if not can_paint(mid, C):
            min_time = mid + 1
        else:
            max_time = mid
    return min_time % 10000003
```