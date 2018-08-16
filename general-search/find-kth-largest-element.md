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



