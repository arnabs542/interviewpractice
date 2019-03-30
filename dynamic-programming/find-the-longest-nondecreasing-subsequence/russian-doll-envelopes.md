#### Russian Doll Envelopes

> You have a number of envelopes with widths and heights given as a pair of integers `(w, h)`. One envelope can fit into another if and only if both the width and height of one envelope is greater than the width and height of the other envelope.
>
> What is the maximum number of envelopes can you Russian doll? \(put one inside other\)
>
> **Note:**  
>  Rotation is not allowed.
>
> **Example:**
>
> ```
> Input: [[5,4],[6,4],[6,7],[2,3]]
> Output: 3 
>
> Explanation: 
> The maximum number of envelopes you can Russian doll is 3 ([2,3] => [5,4] => [6,7]).
> ```

##### Dynamic Programming:

We could approach this same way we approach the LIS problem:

```py
def maxEnvelopes(envelopes: "List[List[int]]") -> int:
    n = len(envelopes)
    if n == 0:
        return 0

    envelopes.sort(key=lambda x: (x[0]))        
    lis = [1] * n

    for i in range(1, n):
        for j in range(i):
            if envelopes[i][0] > envelopes[j][0] and envelopes[i][1] > envelopes[j][1]:
                lis[i] = max(lis[i], lis[j] + 1)

    return max(lis)
```

Since, we're not given any restrictions on the ordering, we can sort the array. We pick one feature to sort \(weight in the above code\), and then iterate through the dolls, checking for the longest subsequence that can be formed among all previous dolls that can be fit into the current one.

Running time is $$\small \mathcal O(n^{2})$$, and space is $$\small \mathcal O(n)$$.

For large inputs, we obviously want use the $$\small \mathcal O(n\log{n})$$ approach. The difficulty in applying that directly to this problem is that we have two factors to compare, instead of a single number. In order to use the binary search method, we need to some how take care of a factor independently:

```py
def bin_search(lis: "List[List[int]]", doll: "List[int]"):
    lo, hi = 0, len(lis) - 1
    while lo < hi:
        mid = (lo + hi) >> 1
        if lis[mid][1] < doll[1]:
            lo = mid + 1
        else:
            hi = mid
    return lo

def maxEnvelopes(envelopes: List[List[int]]) -> int:
    n = len(envelopes)
    if n == 0:
        return 0

    envelopes.sort(key=lambda x: (x[0], -x[1]))        
    lis = []

    for doll in envelopes:
        if not lis or doll[0] > lis[-1][0] and doll[1] > lis[-1][1]:
            lis.append(doll)
        else:
            idx = bin_search(lis, doll)
            lis[idx] = doll                

    return len(lis)
```

When we sort the dolls, we sort by widths, then break ties by reverse heights. In other words, if two dolls have the same width, the higher doll will appear first. The reason for doing so is that by first sorting by widths, we know that there will never be an issue with a previous doll being too wide for the current doll. However, by sorting by reverse height, we ensure that dolls strictly become more "stuffable".  

Another way to understand the reverse secondary sort is via case analysis:

1. Since we sort by widths, every doll will be at least as wide as all the dolls in the chain

2. Since we reverse sort heights, every doll will be at most as tall as the dolls in the chain with the same width

   a. Suppose `idx = len(chain)`:

   1. The current doll is shorter than/equal to a previously seen doll height-wise. However, since weight is normally sorted, this doll is guaranteed to be wider than all dolls in `chain[:idx]`

   b. Suppose `idx < len(chain)`:

   1. The current doll is shorter than/equal to a previously seen doll height-wise. However, since weight is normally sorted, this doll is guaranteed to be wider than all dolls in `chain[:idx]`

We need to find the leftmost insertion point to deal with duplicates, such as`[[1,1,],[1,1],[1,1]]`. If we don't find the l.i.p, then we'll end up appending the duplicates after each other, giving us the wrong answer.

Running time is now $$\small \mathcal O(n \log{n})$$, space remains $$\small \mathcal O(n)$$.

