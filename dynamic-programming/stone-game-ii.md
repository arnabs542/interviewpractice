#### Stone Game II

> Alex and Lee continue their games with piles of stones.  There are a number of piles arranged in a row, and each pile has a positive integer number of stones `piles[i]`.  The objective of the game is to end with the most stones. 
>
> Alex and Lee take turns, with Alex starting first.  Initially, `M = 1`.
>
> On each player's turn, that player can take all the stones in the first `X` remaining piles, where `1 <= X <= 2M`.  Then, we set `M = max(M, X)`.
>
> The game continues until all the stones have been taken.
>
> Assuming Alex and Lee play optimally, return the maximum number of stones Alex can get.
>
> Example 1:
> ```
> Input: piles = [2,7,9,4,4]
> Output: 10
> Explanation:  If Alex takes one pile at the beginning, Lee takes two piles, then Alex takes 2 piles again. Alex can get 2 + 4 + 4 = 10 piles in total. If Alex takes two piles at the beginning, then Lee can take all three piles left. In this case, Alex get 2 + 7 = 9 piles in total. So we return 10 since it's larger. 
> ```

##### Solution

As with these type of min-max/optimization problems, it's helpful to first establish a recursive solution. 

We can simulate exactly how the game will play out by simply trying all possible combination of moves. When it's Alex's turn to take a stone, we'll add a positive number to the score. When it's Lee's turn to pick a stone, we'll subtract a number to the score. The resulting delta can be used to recalculate the two scores:

```py
def stoneGameII(piles: "List[int]") -> int:

    def helper(piles, M):
        if not piles:   # out of stones
            return 0
        if 2*M >= len(piles):   # pick up all stones
            return sum(piles)
        best_score = float('-inf')
        for x in range(1, 2*M+1):
            best_score = max(best_score, sum(piles[:x] - helper(start+x, max(x, M)))
        return best_score
    
    delta = helper(0, 1)
    return (sum(piles) + delta) // 2
```

We notice that we'll probably end up redoing a lot of subproblems with the same `piles` and `M`. This implies we should use memoization to save time. However, we can't just hash a list, so instead of passing an entire list, we'll just pass the current starting index:

```py
def stoneGameII(piles: "List[int]") -> int:
    
    memo = {}
    def helper(start, M):
        if (start, M) in memo:
            return memo[(start, M)]
        if start >= len(piles):
            return 0    # Out of stones
        if len(piles) - start <= 2*M:   # Can pick up all stones
            return sum(piles[start: ])
        best_score = float('-inf')
        for x in range(1, 2*M + 1):
            best_score = max(best_score, sum(piles[start:start+x]) - helper(start + x, max(x, M)))
        memo[(start, M)] = best_score
        return memo[(start, M)]

    helper(0, 1)
    return (sum(piles) + memo[(0, 1)]) // 2
```

The final tweak we can make to this algorithm is construct a prefix, or in this a case, a suffix sum array so we don't constatly need to do `sum(piles[i:j])`.

```py
def stoneGameII(piles: List[int]) -> int:
    
    for i in range(len(piles) - 2, -1, -1):
        piles[i] += piles[i+1]
    
    memo = {}
    def helper(start, M):
        if (start, M) in memo:
            return memo[(start, M)]
        if start >= len(piles):
            return 0    # Out of stones
        if len(piles) - start <= 2*M:   # Can pick up all stones
            return piles[start]
        best_score = float('-inf')
        for x in range(1, 2*M + 1):
            best_score = max(best_score, piles[start] - piles[start+x] - helper(start + x, max(x, M)))
        memo[(start, M)] = best_score
        return memo[(start, M)]

    helper(0, 1)
    return (piles[0] + memo[(0, 1)]) // 2
```

There are $\small \mathcal O(n)$ start positions, and $\small M$ ranges between 1 and $\small \log{n}$. Overall running time is therefore $\small \mathcal O(n \log{n})$.