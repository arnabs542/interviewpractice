#### Largest Rectangle in Histogram

> Given n non-negative integers representing the histogram's bar height where the width of each bar is `1`, find the area of largest rectangle in the histogram.
>
>![example](assets\largest_rectangle_histogram.png)

##### Solution

While this problem may appear very similar to the `Max Trapped Water` problem, there is a distinct difference. Whereas in the previous problem, we simply cared about the boundaries, in this problem, we are limited by the smallest value in the boundary. For example, given an array `heights = [1, 4, 2, 5, 6, 3, 2, 6, 6, 5, 2, 1, 3]`, the heights `4` and `3` don't actually matter, because the minimum height within those two boundes is actually `1`, and that is therefore the maximum height of the bounded rectangle.

