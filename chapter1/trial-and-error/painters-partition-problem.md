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

##### Recursion:

```py
def paint(A, B, C):

    def helper(start, divs, partitions, t_time, cur_sum):
        if not divs:
            partitions.append(sum(C[start:]) * B)
            t_time[0] = min(t_time[0], max(partitions))
            partitions.pop()
            return
        for i in range(start, len(C) - divs):
            cur_sum += C[i]
            partitions.append(cur_sum * B)
            helper(i+1, divs-1, partitions, t_time, 0)
            partitions.pop()

    t_time = [float("inf")]
    A = min(len(C), A)
    helper(0, A - 1, [], t_time, 0)
    return t_time[0] % 10000003
```

This problem could also theoretically be solved via brute force, but in reality this quickly times out. Partitions can be counted via Striling numbers of the second kind \([https://en.wikipedia.org/wiki/Stirling\_numbers\_of\_the\_second\_kind\](https://en.wikipedia.org/wiki/Stirling_numbers_of_the_second_kind%29\).

The algorithm is still worth a look at. We simply iterate through the array, splitting along each valid split point \(we must leave enough elements for the rest of the painters\), then recursively call the helper function to deal with the rest. When we have assigned sections to all painters, we then look at what the largest section is, and that will be the time required to paint according to the current division.

##### Dynamic Programming

The above recurrence can be modeled as such: the time taken to paint $$\small n$$ boards with $$\small k$$ painter is $$\small T(n, k) = \min{}$$

##### Edge Cases:

* Empty fence list
* Number of painters &gt; number of boards



