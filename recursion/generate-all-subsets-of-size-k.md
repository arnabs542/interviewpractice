#### Generate all Subsets of Size k

> Given two integers _n_ and _k_, return all possible combinations of _k_ numbers out of 1 ... _n_.
>
> **Example:**
>
> ```
> Input: n = 4, k = 2
>
> Output:
> [
>   [2,4],
>   [3,4],
>   [2,3],
>   [1,2],
>   [1,3],
>   [1,4],
> ]
> ```

##### Recursion:

```py
def combinations(n, k):
    res = []
    n = [i for i in range(1, n+1)]

    def dfs(idx, cur):
        if len(cur) == k:
            res.append(cur.copy())
            return
        if idx >= len(n):
            return
        cur.append(n[idx])
        dfs(idx+1, cur)
        cur.pop()
        dfs(idx + 1, cur)

    dfs(0, [])
    return res
```

We use the same algorithm we used to generate subsets, except this time we break when our current subset reaches the length indicated by $\small k$. Running time should be bounded by $\small \mathcal O(k* 2^{n})$. Space is bounded by $\small \mathcal O(\binom{n}{k} * k)$.

##### Recursion \(Case analysis\):

```py
def helper(nums, N, start, k):
    if N == k:
        return [nums[start:]]
    if k == 0:
        return [[]]

    ans = []

    pick = nums[start]

    for i in helper(nums, N - 1, start + 1, k - 1):
        ans.append([pick] + i)

    for i in helper(nums, N - 1, start + 1, k):
        ans.append(i)

    return ans


def combinations(n, k):
    nums = list(range(1, n + 1))
    return helper(nums, n, 0, k)
```

Another recursive method is to frame the problem in a more focused manner. There are two possibilities for a subset - it does not contain 1, or it does contain 1. In the first case, we return all subsets of size $\small k$ of $\small \{2,3,...,n\}$; in the second case, we return compute all $\small k-1$ sized subsets of $\small \{2,3,...,n\}$ and add 1 to each of them. 

In the helper function above, the arguments `nums, N, start, k` respectively represent the input array, the size of the current subarray, the starting element of the current subarray, and $\small k$. If the current subarray starting from `start` is exactly size $\small k$, then return the entire subarray, since we can't shrink it anymore. Otherwise, we generate all subarrays containing and excluding the current element.

