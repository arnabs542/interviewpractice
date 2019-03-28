#### Find the Longest Nondecreasing Subsequence

> The problem of finding the longest nondecreasing subsequence in a sequence of integers has implications to many disciplines, including string matching and analyzing card games. As a concrete instance, the length of a longest nondecreasing subsequence for the array $$\small <0, 8, 4, 12, 2, 10, 6, 14, 1, 9>$$ is 4. Note there are multiple longest nondecreasing subsequences, e.g., $$\small <0,4,10,14>$$ and $$\small <0,2,6,9>$$. Also note that the elements of non-decreasing subsequence are not required to immediately follow each other in the original sequence.

##### Dynamic Programming \(Iteration\):

```cpp
def longest_nondecreasing_subsequence_length(A):
    if not A:
        return 0

    longest_sequence_with_num = [1] * len(A)
    max_length = 1

    for i in range(1, len(A)):
        for j in range(i):
            if A[i] >= A[j]:
                longest_sequence_with_num[i] = max(longest_sequence_with_num[i], longest_sequence_with_num[j] + 1)
                max_length = max(max_length, longest_sequence_with_num[i])

    return max_length
```

Intuitively, if we have processed the initial set of entries of the input array, we should be able to use those results when dealing with the next entry. Concretely, if we know the longest subsequences of `A[0:5]`, then the longest possible subsequence that ends at `A[5]` would be one more than the longest subsequences ending at `A[0:5]` whose value is smaller than or equal to `A[5]`.

The running time is $$\small \mathcal O(n^{2})$$, and space is $$\small \mathcal O(n)$$.

We can also reconstruct a single longest nondecreasing subsequence by working backwards from our final array:

```py
def longest_nondecreasing_subsequence_length(A):
    longest_sequence_with_num = [1] * len(A)
    max_length, idx = 1, 0

    for i in range(1, len(A)):
        for j in range(i):
            if A[i] >= A[j]:
                longest_sequence_with_num[i] = max(longest_sequence_with_num[i], longest_sequence_with_num[j] + 1)
                if longest_sequence_with_num[i] > max_length:
                    max_length = longest_sequence_with_num[i]
                    idx = i

    # Reconstruct a subsequence
    subsequence = []
    for i in range(max_length):
        subsequence.append(A[idx])
        next_idx = idx - 1
        while next_idx >= 0 and not (A[next_idx] <= A[idx] and longest_sequence_with_num[next_idx] 
                                     == longest_sequence_with_num[idx] - 1):
            next_idx -= 1
        idx = next_idx

    return subsequence[::-1]
```

##### Binary Search:

```py
def binary_search(A, val):
    lo, hi = 0, len(A) - 1
    while lo < hi:
        mid = (lo + hi) >> 1
        if A[mid] <= val:
            lo = mid + 1
        else:
            hi = mid
    return lo

def longest_nondecreasing_subsequence_length(A):
    if not A:
        return 0

    subsequence = []

    for num in A:
        if not subsequence or num >= subsequence[-1]:
            subsequence.append(num)
        else:
            idx = binary_search(subsequence, num)
            subsequence[idx] = num

    return len(subsequence)
```

The first thing to note is that this method doesn't actually store the final subsequence. Instead, it builds a dummy sequence with the final length equal to the longest nondecreasing subsequence.

As we iterate through the array, we look at where each element would fit in our subsequence. If it's the largest element we've seen so far, then we simply append it to the end. Otherwise, we find the first item it would replace \(the first item larger than or equal to the current item\) and replace that item with this one. The reason for doing so is that if this item is smaller than the item we just replaced, then we have a better chance of constructing a longer chain in the future, since we're appending to a smaller item.

Running time is now $$\small \mathcal O(n \log{n})$$.



