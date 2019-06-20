#### Largest Rectangle in Histogram

> Given n non-negative integers representing the histogram's bar height where the width of each bar is `1`, find the area of largest rectangle in the histogram.
>
>![](/assets/largest_rectangle_histogram.png)

##### Solution

While this problem may appear very similar to the `Max Trapped Water` problem, there is a distinct difference. Whereas in the previous problem, we simply cared about the boundaries, in this problem, we are limited by the smallest value in the boundary. For example, given an array `heights = [1, 4, 2, 5, 6, 3, 2, 6, 6, 5, 2, 1, 3]`, the heights `4` and `3` don't actually matter, because the minimum height within those two boundes is actually `1`, and that is therefore the maximum height of the bounded rectangle.

What the difference means is that we can't frame the problem as finding every pair `(i, j)` and testing the maximum area. Instead, we need to reframe the problem as how to efficiently find the largest rectangle which includes the `i`th building, and has a height of `heights[i]`.

We could start at each building `i`, and expand both directions until we hit a building shorter than it. Since `i` ranges from `0` to `n-1`, this takes $\small \mathcal O(n^{2})$ time. However, we can improve on this. Suppose we iterate over buildings from left to right. When we process building `i`, we do not know how far to the right the largest rectangle it supports goes. However, we do know that the largest rectangles supported by earlier buildings whose height is greater than `heights[i]` cannot extend past `i`, since building `i` "blocks" these rectangles. In other words, any rectangle that contains building `i`, in which `heights[i]` is the lowest height, has its height bounded by `heights[i]`. 

We will iterate over our array, considering each index to be the right boundary. That is, for each item in our array, we will calculate the area of the best rectangle that can be made whose right boundary is the current index.

Our stack will represent the potential left boundaries of rectangles being considered, and we will pop elements off that no longer help us make our decision.

The clearest way to see what is happening is to look at the series of operations as we process the example array given above.

```
stack   | last height | left | right | best area
------------------------------------------------
[0]           1          -1      0         0
[0, 1]        3          -1      1         0
[0]           1           0      2         3           
[0, 2]        2           0      2         3
[0, 2, 3]     5          -1      3         3
[0, 2]        2           2      4         5
[0]           1           0      4         6           
[]            -          -1      4         6
```

After looking at each element, we append it to our stack. As long as the last element of our stack represents a rectangle taller than the current one, we pop that element and use it as our height. With each pop, the left boundary will be shifted by one, incrementing the width of our potential rectangle. Finally, we update the running maximum, if necessary, to be the product of our new height and width.

```py
def largest_rectangle(array):
    stack = []
    max_area = 0

    # Append a zero to handle cases of non-decreasing sequences.
    array.append(0)

    for right_index, rect in enumerate(array):
        while stack and rect < array[stack[-1]]:
            height = array[stack.pop()]

            left_index = stack[-1] if stack else -1
            width = right_index - (left_index + 1)

            max_area = max(max_area, height * width)

        stack.append(right_index)

    return max_area
```

The time and space complexity of this algorithm will be $\small \mathcal O(N)$, since we make a single pass over our array and maintain a stack of size at most $\small N$.