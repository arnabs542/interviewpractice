#### Find the Closest Entries in Three Sorted Arrays

> Design an algorithm that takes three sorted arrays and returns one entry from each such that the minimum interval containing these three entries is as small as possible. For example, if the three arrays are \[5,10,15\], \[3,6,9,12,15\], and \[8,16,24\], then 15,15,16 lie in the smallest possible interval.

##### Code:

```
def find_closest_elements_in_sorted_arrays(sorted_arrays):
    min_distance_so_far = float('inf')
    # Stores array iterators in each entry
    iters = bintrees.RBTree()
    for idx, sorted_array in enumerate(sorted_arrays):
        it = iter(sorted_array)
        first_min = next(it, None)
        if first_min is not None:
            iters.insert((first_min, idx), it)

    while True:
        min_value, min_idx = iters.min_key()
        max_value = iters.max_key()[0]
        min_distance_so_far = min(min_distance_so_far, max_value - min_value)
        it = iters.pop_min()[1]
        next_min = next(it, None)
        # Return if some array has no remaining elements
        if next_min is None:
            return min_distance_so_far
        iters.insert((next_min, min_idx), it)
```

##### Explanation:

The brute force solution would be to run three nested for loops. However this, doesn't take advantage of the fact that all the arrays are sorted. For example, the interval containing \[5,3,16\] must be smaller than the interval containing \[5,3,24\], and there's no point for the loop to continue past 24, since the intervals will only end up larger.

Let's suppose that we begin with the the triple consisting of the smallest elements from each array. Let $$\small s$$ be the minimum value in the triple and $$\small t$$ be the maximum value in the triple. Then the smallest interval with left endpoint $$\small s$$ containing elements from each array must be $$\small [s,t]$$, since the remaining two values are the minimum possible.

Now remove $$\small s$$ from the triple and bring the next smallest element from the array it belongs to into the triple. Let $$\small s'$$ and $$\small t'$$be the next minimum and maximum values in the new triple. Observe $$\small [s', t']$$ must be the smallest interval whose left endpoint is $$\small s'$$: the other two values are the smallest values in the corresponding arrays that are greater than or equal to $$\small s'$$. By iteratively examining and removing the smallest element from the triple, we compute the minimum interval starting at that element. Since the minimum interval containing elements from each array must begin with the element of _some_ array, we are guaranteed to encounter the minimum element.

In other words, if we have an interval, there is no point in moving the largest element, since that will only increase the interval. We could move the middle elements, but best case the interval size doesn't change, and worse case we end up with a larger interval. Only by moving the smallest element can we attempt to get a smaller interval.

In the above generalized solution for _k_ arrays, it's important to note that our keys for the tree is a tuple consisting of the element and the index of its array. The reason we can't simply use the element as the k is that Red-black trees don't accept duplicate keys, so we have to include the unique index of the array each element comes from to ensure that duplicate numbers don't get lost.

Time complexity is $$\small \mathcal O(n \log{k})$$, where $$\small n$$ is the total number of elements in the $$\small k$$ arrays. 

