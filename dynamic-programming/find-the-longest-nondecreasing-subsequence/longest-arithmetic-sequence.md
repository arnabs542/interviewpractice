#### Longest Arithmetic Sequence

> Given an array `A` of integers, return the **length** of the longest arithmetic subsequence in `A`.
>
> Recall that a _subsequence_ of `A` is a list `A[i_1], A[i_2], ..., A[i_k]` with `0 <= i_1 < i_2 < ... < i_k <= A.length - 1`, and that a sequence `B` is _arithmetic_ if `B[i+1] - B[i]` are all the same value \(for `0 <= i < B.length - 1`\).
>
> **Example 1:**
>
> ```
> Input: [3,6,9,12]
> Output: 4
> Explanation: 
> The whole array is an arithmetic sequence with steps of length = 3.
> ```
>
> **Example 2:**
>
> ```
> Input: [9,4,7,2,10]
> Output: 3
> Explanation: 
> The longest arithmetic subsequence is [4,7,10].
> ```
>
> **Example 3:**
>
> ```
> Input: [20,1,15,3,10,5,8]
> Output: 4
> Explanation: 
> The longest arithmetic subsequence is [20,15,10,5].
> ```
>
> **Note:**
>
> 1. `2 <= A.length <= 2000`
> 2. `0 <= A[i] <= 10000`

##### Dynamic Programming:

```py
def longestArithSeqLength(A: "List[int]") -> int:

    if len(A) <= 2:
        return len(A)

    lengths = [1] * len(A)
    diffs = [{} for i in range(len(A))]

    for i in range(1, len(A)):
        for j in range(i):
            diff = A[i] - A[j]
            if diff in diffs[j]:
                lengths[i] = max(lengths[i], diffs[j][diff] + 1)
            else:
                lengths[i] = max(lengths[i], 2)

            diffs[i][diff] = diffs[j].get(diff, 1) + 1

    return max(lengths)
```

The key idea of the above solution is build off the LIS problem. The modification here is that for every index, we need to store what arithmetic sequences end at that index, and how long they are. In other words, `diffs[index][diff]` stores the length of the arithmetic sequence ending at index with difference `diff`.

Since any two elements form a arithmetic sequence, our inner loop checks if there is a sequence ending at `A[j]` with diff. If not, then `A[i]` can form a new sequence with `A[j]` of length 2. If there is already a sequence, then we can just attach `A[i]` to the end of the sequence at `A[j]`.

Let `A=[20,1,15,3,10,5,8]`. Then diff would look like this:

```
[{}, {-19: 2}, {-5: 2, 14: 2}, {-17: 2, 2: 2, -12: 2}, {-10: 2, 9: 2, -5: 3, 7: 2}, 
{-15: 2, 4: 2, -10: 2, 2: 3, -5: 4}, {-12: 2, 7: 2, -7: 2, 5: 2, -2: 2, 3: 2}]
```

For `A[4]`, there are 4 arithmetic sequences it can form: `[20, 10], [1, 10], [20, 15, 10], [3,10]`. The longest is `[20,15,10]`, which has a length of `3` and a difference of `-5`.

Runtime is $$\small \mathcal O(n^{2})$$, same for space.

