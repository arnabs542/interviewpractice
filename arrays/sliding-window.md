#### Sliding Window

The sliding window technique is a very common technique used in array or string problems, since it can help reduce the time complexity from $$\small \mathcal O(n^{2})$$ to $$\small \mathcal O(n)$$.

**There are two types of sliding window:**

1. Fixed window length k: the length of the window is fixed and it asks you to find something in the window such as the maximum sum of all windows, the maximum or median number of each window. Usually we need kind of variables to maintain the state of the window, some are as simple as a integer or it could be as complicated as some advanced data structure such as list, queue or deque.
2. Two pointers + criteria: the window size is not fixed, usually it asks you to find the subarray that meets the criteria, for example, given an array of integers, find the number of subarrays whose sum is equal to a target value. In this case, we usually increase the window until some criteria is broke, then we try to shrink the window until the criteria is met again before continuing.



