#### Container With Most Water

> Given `n` non-negative integers `a1, a2, ..., an`, where each represents a point at coordinate `(i, ai)`. n vertical lines are drawn such that the two endpoints of line `i` is at `(i, ai)` and `(i, 0)`. Find two lines, which together with x-axis forms a container, such that the container contains the most water.
>
> For example, given the array `[1,8,6,2,5,4,8,3,7]`, the answer is `49`, which is given by bounds at index `1` and `8`. 

##### Solution

There are two parameters that dictate how much water we can hold: the height of the container and the width. Ideally, we would be able to increase both, but if we had to decrease a parameter, we would want to compensate by increasing the other.

That realization is the key to solving this problem. Let us start with the first and last heights - this gives us the greatest width possible. Any other pair will have a smaller width, but we should try to increase the height of smallest current bound to compensate. In other words, we will greedily trade width for height.

```py
def calculate_largest_rectangle(heights):
    i, j, max_water = 0, len(heights) - 1, 0
    while i < j:
        width = j - i
        max_water = max(max_water, width * min(heights[i], heights[j]))
        if heights[i] > heights[j]:
            j -= 1
        else:   # heights[i] <= j
            i += 1
    return max_water
```
