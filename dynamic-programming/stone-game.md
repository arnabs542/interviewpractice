#### Stone Game

> Alex and Lee play a game with piles of stones.  There are an even number of piles **arranged in a row**, and each pile has a positive integer number of stones `piles[i]`.
>
> The objective of the game is to end with the most stones.  The total number of stones is odd, so there are no ties.
>
> Alex and Lee take turns, with Alex starting first.  Each turn, a player takes the entire pile of stones from either the beginning or the end of the row.  This continues until there are no more piles left, at which point the person with the most stones wins.
>
> Assuming Alex and Lee play optimally, return `True` if and only if Alex wins the game.
>
> **Example 1:**
>
> ```
> Input: [5,3,4,5]
> Output: true
> Explanation: 
>
> Alex starts first, and can only take the first 5 or the last 5.
> Say he takes the first 5, so that the row becomes [3, 4, 5].
> If Lee takes 3, then the board is [4, 5], and Alex takes 5 to win with 10 points.
> If Lee takes the last 5, then the board is [3, 4], and Alex takes 4 to win with 9 points.
> This demonstrated that taking the first 5 was a winning move for Alex, so we return true.
> ```

##### Brute Force:

```py
def stoneGame(piles):
    """
    :type piles: List[int]
    :rtype: bool
    """
    def helper(alex, lee, turn, head, tail):
        if head > tail:
            return alex > lee
        if turn == 0:
            if helper(alex+piles[head], lee, turn ^ 1, head + 1, tail):
                return True
            if helper(alex+piles[tail], lee, turn ^ 1, head, tail - 1):
                return True
        else:
            if helper(alex, lee+piles[head], turn ^ 1, head + 1, tail):
                return True
            if helper(alex, lee+piles[tail], turn ^ 1, head, tail - 1):
                return True
    
    return helper(0, 0, 0, 0, len(piles) - 1)
```

The brute force solution is to simply simulate all possible moves - try picking from the head first, and if that doesn't work, try picking from the tail. 

The run time is $$\small \mathcal O(2^n)$$, and quickly times out.

##### DP:

```py
def stoneGame(self, piles):

    n = len(piles)
    dp= [[0] * n for _ in range(n)]

    for i in range(n):
        dp[i][i] = piles[i]

    for dist in range(1, n):
        for start in range(n - dist):
            dp[start][start+dist] = max(piles[start] - dp[start+1][start+dist], 
                                        piles[start+dist] - dp[start][start+dist-1])

    return dp[0][-1] > 0
```



