#### Find the Min and Max Simultaneously

To the find min and the max in an array we can simply iterate over it twice, once time looking for the min and the next time looking for the max. Doing it like so requires a total of $$\small 2(n-1)$$ comparisons. However, in situations where comparisons themselves are very expensive, e.g., a comparison may involve a number of nested calls or the elements being compared may be long strings, we would like to reduce the number of comparisons needed to find the min and max elements.

##### Code:

```py
def find_min_max(A):

    def min_max(a, b):
        return MinMax(a, b) if a < b else MinMax(b, a)

    if len(A) <= 1:
        return MinMax(A[0], A[0])

    global_min_max = min_max(A[0], A[1])
    # Process two elements at a time
    for i in range(2, len(A) - 1, 2):
        local_min_max = min_max(A[i], A[i + 1])
        global_min_max = MinMax(min(global_min_max.smallest, local_min_max.smallest),
                                max(global_min_max.largest, local_min_max.largest))

    # If there is odd number of elements in the array, we need to compare last element
    if len(A) % 2:
        global_min_max = MinMax(min(global_min_max.smallest, A[-1]),
                                max(global_min_max.largest, A[-1]))

    return global_min_max
```

##### Explanation:

The main idea behind the above solution is to use a tournament style comparison guide. Suppose we are looking for the smallest element. If the array was divided up into groups, and someone told us what the smallest element in each group were, we would not waste anytime with the rest of the elements and directly start with the smallest element from each group.

This is exactly how we proceed. We process the array in pairs of elements, and we distinguish the min and max from each pair first - this will give us $$\small n/2$$ candidates for min and $$\small n/2$$ candidates for max at the cost of $$\small n/2$$ comparisons. It takes $$\small n/2 -1$$ comparisons to find the min from the main candidates and $$\small n/2-1$$ candidates to find the max, yielding a total of $$\small 3n/2 -2$$ comparisons required, a drastic improvement over the naive method.

However, our improved method requires $$\small \mathcal O(n)$$ storage when implemented naively, since we need to store the candidates in arrays. Instead, we avoid using extra space by implementing it in a streaming fashion, by maintaining candidate min and max as we process successive pairs.

Overall time complexity is $$\small \mathcal O(n)$$ and space complexity is $$\small \mathcal O(1)$$.

##### Quick proof that the lower bound of $$\lceil \frac{3n}{2} \rceil - 2$$ comparisons is the worst case to find both the maximum and minimum of n numbers:

Each comparison can reduce both the potential minimums and maximums by one. Note that this is now always the case if we make two comparisons to infer that $$\small a<b$$ and $$\small a<c$$, we have excluded two elements from the potential minimums \($$\small b, c)$$, but only one from the potential maximums $$\small (a)$$.

We can optimize by splitting the input in pairs and comparing each pair. After $$\small n/2$$ comparisons, we have reduced the potential minimums and potential maximums to $$\small n/2$$ each. Furthermore, those two sets are disjoint so now we have two problems, one minimum and one maximum, each of size $$\small n/2$$. The total number of comparisons is:


$$
 n/2 + 2(n/2 - 1) = n/2 + n - 2 = 3n/2 - 2
$$


This assumes that $$\small n$$ is even. If $$\small n$$ is odd we need one additional comparison in order to determine whether the last element is a potential minimum or maximum. Hence the ceiling.

