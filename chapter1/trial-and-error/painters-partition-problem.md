#### Painter's Partition Problem

> You have to paint N boards of length `{A0,A1,A2,A3…AN-1}`. There are `K` painters available and you are also given how much time a painter takes to paint `1 unit` of board. You have to get this job done `as soon as possible` under the constraints that **any painter will only paint contiguous sections of board.**
>
> * 2 painters cannot share a board to paint. That is to say, a board cannot be painted partially by one painter, and partially by another.
> * A painter will only paint contiguous boards. Which means a configuration where painter 1 paints board 1 and 3 but not 2 is invalid.

##### Binary Search:

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
            #print(cur_work, time, painters)
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

The search space is between the minimum possible time 0 \(no boards\) and maximum possible time \(1 painter, sum all boards\). We use binary search to find the smallest possible time under which all fences can be painted. 

The trick was in the implementation of the `can_paint` function. I had initially sorted the array to greedily assign the boards, which was incorrect. The key constraint is that the painters can only work on contiguous sections of the board. That means in the final solution, each painter will be responsible for a particular section of the boards, meaning they have a starting point for their section. Once chosen, then can only paint in the right direction. Since some painter must start with the first board, we will just begin by assigning the boards to a painter until their current assigned work reaches the maximum limit. Then we start assigning the rest of the boards to the next painter. If there are no painters remaining and we haven't finished all the boards, we know it's not possible to finish with the current time and continue our binary search. Running time is $$\small \mathcal O(n \log{c})$$, where $$\small c$$ is the sum of the boards. Space complexity is constant. 

