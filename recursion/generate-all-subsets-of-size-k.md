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

We use the same algorithm we used to generate subsets, except this time we break when our current subset reaches the length indicated by $$\small k$$. Running time should be bounded by $$\small \mathcal O(k*\binom{n}{k})$$. Space is bounded by $$\small \mathcal O(\binom{n}{k} * k)$$.

