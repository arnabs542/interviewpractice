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

