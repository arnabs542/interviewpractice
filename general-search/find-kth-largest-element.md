#### Find kth Largest Element

> Find the **k**th largest element in an unsorted array. Note that it is the kth largest element in the sorted order, not the kth distinct element.
>
> **Example 1:**
>
> ```
> Input:
> [3,2,1,5,6,4] 
> and k = 2
>
> Output: 5
> ```
>
> **Example 2:**
>
> ```
> Input:
> [3,2,3,1,2,4,5,5,6] 
> and k = 4
>
> Output: 4
> ```

A naive way to approach this problem would be to simply sort the array, then return the index at the $$\small k^{th}$$ index. This results in at best $$\small \mathcal O(n \log(n))$$ time complexity. Space complexity differs depending on sorting algorithm used. However, this is not actually idea, because there is no reason to actually do the work of sorting all elements. If $$\small k << n$$, we don't really care about the order of all elements larger that the $$\small k^{th}$$ element, and therefore we shouldn't waste time putting those in order.

We could also use a heap to find the correct element, improving our running time to $$\small \mathcal O(n \log(k))$$ and the cost of $$\small \mathcal O(k)$$ space, but again, we're doing a lot more work than we need to. The heap not only gives the $$\small k^{th}$$ element, but it actually computes the $$\small k$$ largest elements in sorted order when we're only asked for a single element.

##### Code \(Simple version\):

```py
def find_kth_largest(k, A):

    idx = random.randint(0, len(A) - 1)
    smaller = [e for e in A if e < A[idx]]
    equal = [e for e in A if e == A[idx]]
    larger = [e for e in A if e > A[idx]]

    if len(larger) >= k:
        return find_kth_largest(k, A)
    elif len(equal) >= k - len(larger):
        return equal[0]
    else:
        return find_kth_largest(k - len(equal) - len(larger), smaller)
```

##### Explanation:

The code is very poorly optimized, but it illustrates the underlying idea very well. To find the $$\small k$$th largest element in-pace without completely sorting the array we can select an element at random \(the "pivot"\) and parition the remaining entries into those greater than the pivot and those less than the pivot. If there are exactly $$\small k-1$$ elements smaller than the pivot, then the pivot is the answer. If there are more than $$\small k-1$$ elements greater than the pivot, we can discard elements less than or equal to the pivot - the $$\small k$$-largest element must be greater than the pivot. If there are less than $$\small k-1$$ elements greater than the pivot, we can discard elements greater than or equal to the pivot.

##### Code \(Optimized version\):

```py
def find_kth_largest(k, A):

    def find_kth(comp):
        # partition A[left:right + 1] around pivot_idx, returns the new 
        #indice of the pivot, new_pivot_idx, after partition
        # After partitioning, A[left:new_pivot_idx] contains elements that are "greater than" the pivot,
        # and A[new_pivot_idx + 1:right + 1] contains elements that are "less than" the pivot
        #
        # Note: "greater than" and "less than" are defined by the comp object
        #
        # Returns the new index of the pivot element after partition
        def partition_around_pivot(left, right, pivot_idx):
            pivot_value = A[pivot_idx]
            new_pivot_idx = left
            A[pivot_idx], A[right] = A[right], A[pivot_idx]
            for i in range(left, right):
                if comp(A[i], pivot_value):
                    A[i], A[new_pivot_idx] = A[new_pivot_idx], A[i]
                    new_pivot_idx += 1
            A[right], A[new_pivot_idx] = A[new_pivot_idx], A[right]
            return new_pivot_idx

        left, right = 0, len(A) - 1
        while left <= right:
            print(A)
            # Generates a random integer in [left, right]
            pivot_idx = random.randint(left, right)
            new_pivot_idx = partition_around_pivot(left, right, pivot_idx)
            print(A)
            if new_pivot_idx == k - 1:
                return A[new_pivot_idx]
            elif new_pivot_idx > k - 1:
                right = new_pivot_idx - 1
            else:    # new_pivot_idx < k - 1
                left = new_pivot_idx + 1

    return find_kth(operator.gt)
```

##### Explanation:

The simple implementation was bad because it initialized basically an entire new array at each recursion level. We can avoid that by performing that partitioning in place as above.

We begin by random generating an index value from within the inclusive left and right bounds. The partition algorithm above makes a few changes that makes the boundary updates significantly easier. First, the array is reverse partitioned, i.e. all of the elements grater than the pivot value comes first. This makes it so that we don't have to do extra subtraction to figure out what position the new index is in, which can lead to lots of off-by-one errors. Next, instead of the color sort program, we don't build the equal or smaller subarray. Instead, we first stash the pivot value all the way to the right, and then as we process the array, we are only concerned with finding elements strictly greater than the pivot value. The final `new_pivot_idx`location is exactly where the pivot\_value wille` `

