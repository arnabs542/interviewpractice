#### Find the Length of a Longest Contained Interval

> Write a program which takes as input a set of integers represented by an array, and returns the size of a largest subset of integers in the array having the property that if two integers are in the subset, then so are all the integers between them. For example, if the input is $$\small <3,-2,7,9,8,1,2,0,-1,5,8>$$, the largest sub subset is $$\small \{-2,-1,0,1,2,3\}$$, so you should return 6.

##### Code:

```py
def longest_contained_range(A):
    elements = set(A)
    max_interval_size = 0
    while elements:
        a = elements.pop()
        lower_bound, upper_bound = a - 1, a + 1
        # Find lower range
        while lower_bound in elements:
            elements.remove(lower_bound)
            lower_bound -= 1
        # Find upper range
        while upper_bound in elements:
            elements.remove(upper_bound)
            upper_bound += 1

        max_interval_size = max(max_interval_size, upper_bound - lower_bound - 1)

    return max_interval_size
```

##### Explanation:

We could of course sort the array, and traverse through it looking for the longest element. However, the key insight is that we don't need the absolute ordering. Suppose that we know and element $$\small e$$ is in the array, then we simply need to check for its successors and predecessors until they are no longer in the array.

We put all elements into a set, and then while the set is not empty, we take a random element and find its highest successor and lowest predecessor. Those two numbers are enough to give us the range. Since we push and pop each element exactly once, the time complexity is $$\small \mathcal O(n)$$.

