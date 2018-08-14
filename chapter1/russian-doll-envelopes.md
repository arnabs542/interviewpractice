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
> Input: 
> [[5,4],[6,4],[6,7],[2,3]]
> Output: 
> 3 
>
> Explanation: The maximum number of envelopes you can Russian doll is 3
> ([2,3] => [5,4] => [6,7])
> ```

Right away this should seem very similar to the Longest Increasing Subsequence problem. However, a key difference is that order does not need to preserved.

My first attempt is basically an adapted version of the O\(n2\) solution for the LIS problem:

```py
def maxEnvelopes(self, envelopes):

    if not envelopes:
        return 0

    n = len(envelopes)
    max_fit = float("-inf")

    # sort based on w
    envelopes = sorted(envelopes, key=lambda x: x[0])
    num_fit = [1] * n

    for i in range(1, n):
        for j in range(i):
            if envelopes[j][0] < envelopes[i][0] and envelopes[j][1] < envelopes[i][1]:
                num_fit[i] = max(num_fit[i], 1 + num_fit[j])
                max_fit = max(max_fit, num_fit[i])

    return max(max_fit, 1)
```

First we sort the dolls based on w \(can also sort based on h\). We then build an dp array to keep track of the max number of envelopes we can fit up using the doll at `nums[i]`. In the end, we return the max number we find.

However, this solution timed out, suggesting that we should use the $$\small \mathcal O(n \log(n))$$ solution from the LIS problem, which built an array and used binary search to find the inserting points of new elements as we encountered them. The final length of the array would be the longest increasing subsequence, although the array itself would not be the answer.

The difficulty with implementing that solution is there are no two parameters to judge. In the LIS problem, because we were just dealing with numbers, it was easy to insert into the array. For example, suppose the input array was: `[10,9,2,5,3,7,101,18]`. As we come across the 2, binary search will return an index 0, and we can replace the original element there \(9\) with a 2. However, in this problem, suppose that we have the input `[[2,3], [3,2], [4,3]]`. It is difficult to judge if we should replace the `[2,3]` with the `[3,2]`. Furthermore, it is difficult to figure out how to apply binary search in order to figure out the right index.

##### Code:

```py
def maxEnvelopes(self, envelopes):  

    if not envelopes:
        return 0

    # Leftmost insertion
    def b_search(chain, doll):
        lo, hi = 0, len(chain)
        while lo < hi:
            mid = (lo + hi) >> 1
            if chain[mid][1] < doll[1]:
                lo = mid + 1
            else:
                hi = mid
        return lo


    # sort based on w, in case of tie, doll with LARGEST height wins
    envelopes.sort(key=lambda x: (x[0], -x[1]))
    chain = []

    for doll in envelopes:
        idx = b_search(chain, doll)
        if idx == len(chain):
            chain.append(doll)
        else:
            chain[idx] = doll

    return len(chain)
```

##### Explanation:

There are three key points to solving this problem in O\(n log n\) time:

1. Sort by width. Break ties by letting doll with largest height win.

2. When implementing binary search, look for leftmost insertion point.

The intuition behind this problem comes from the realization that binary search works only for direct comparisons. Therefore, if we choose to use height as our distinguisher, we must find a way to deal with width separately.

The reason we use a reverse secondary sort \(the height breaker\) is to avoid stuffing envelopes with same widths but different heights.

For example, suppose we are given an input array like so: `[[2,3],[5,4],[5,6],[6,5]]`. If we sort normally, we may end up with `[[2,3],[5,4],[5,6],[6,5]]`. As we build our sequence, we will end up appending `[5,6]` after `[5,4]`, since our binary search finds position based on height. If we were to use a reverse secondary search, we will end up with `[[2,3],[5,6],[5,4],[6,5]]`. When we come to `[5,4]`, we will end up replacing `[5,6]`, which makes sense, since `[5,4]` has the same width but has a smaller height, making it more likely to be "stuffable".

In other words, we want to make sure each next doll is better than previous dolls.

Another way to understand the reverse secondary sort is via case analysis:

1. Since we sort by widths, every doll will be at least as wide as all the dolls in the chain

2. Since we reverse sort heights, every doll will be at most as tall as the dolls in the chain with the same width

   a. Suppose idx = len\(chain\):

   1.  The current doll is shorter than/equal to a previously seen doll height-wise. However, since weight is normally sorted, this doll is guaranteed to be wider than all dolls in `chain[:idx]      `

   b. Suppose idx &lt; len\(chain\):

   1. The current doll is shorter than/equal to a previously seen doll height-wise. However, since weight is normally sorted, this doll is guaranteed to be wider than all dolls in `chain[:idx]`

We need to find the leftmost insertion point to deal with duplicates, such as` [[1,1,],[1,1],[1,1]]`. If we don't find the l.i.p, then we'll end up appending the duplicates after each other, giving us the wrong answer.

