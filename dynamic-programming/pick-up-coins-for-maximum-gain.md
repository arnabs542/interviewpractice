#### Pick Up Coins for Maximum Gain

> In the pick-up-coins game, and even number of coins are placed in a line. Two players take turns at choosing one coin each - they can only choose from the two coins at the ends of the line. The game ends when all the coins have been picked up. The player whose coins have the higher total value wins. A player cannot pass his turn.
>
> Design an efficient algorithm for computing the maximum total value for the starting player in the pick-up-coins game.

##### Dynamic Programming \(Bottom-up\)

It's important to note that a greedy approach will not work for this problem. Suppose the coins are \[5, 25, 10, 1\]. If the first player chooses 5, the second player will choose 25, and thus win. The first player needs to choose 1 in order to force the second player to expose the 25, and thus lose the game. The fundamental problem with the greedy approach is that it doesn't consider the opportunities created for the second player.

This leads to a key insight - the first player needs to balance selecting high coins with minimizing the coins available to the second player.

```js
def maximum_revenue(coins):

    dp = [[0] * len(coins) for _ in range(len(coins))]

    for length in range(len(coins)):
        for start in range(len(coins)):
            if length == 0:
                dp[start][start] = coins[start]
            elif start + length >= len(coins):
                break
            else:
                dp[start][start + length] = max(coins[start] - dp[start + 1][start + length], 
                                                coins[start + length] - dp[start][start + length - 1])

    return (sum(coins) + dp[0][-1]) // 2
```

The array `dp` keeps track of the maximum difference between the two scores depending on the index bounds. For example, the entry at `dp[i][j]` will store the maximum difference between player one's score and player two's final score if we start the game with `coins[i:j+1]`. A positive score means that at the end, player one will win by that much. A negative score means player one will lose by that much.

When we come across an entry `dp[i][j],` we can choose to take from `coins[i]` or `coins[j]`. If we take from `coins[i]`, then it's the same as player two starting from `dp[i+1][j]`, so the final difference between the scores is `coins[i] - dp[i+1][j]`. The same logic applies for picking `coins[j]` instead.

##### Dynamic Programming \(Top-down\):

The strategy of the players don't change - both seek to balance maximizing their score and minimizing the other's score. Let $$\small R(a,b)$$ be the maximum revenue a player can get when it is his turn to play, and the coins remaining on the table are at indices $$\small a$$ to $$\small b$$, inclusive. Let $$\small C$$ be an array representing the lines of coins, i.e., $$\small C[i]$$ is the value of the $$\small i$$th coin. If the first player selects the coin at $$\small a$$, since the second player plays optimally, the first player will end up with a total revenue of $$\small C[a] + S(a+1,b) - R(a+1,b)$$, where $$\small S(a,b)$$ is the sum of the coins from the positions $$\small a$$ to $$\small b$$, inclusive. If he selects the coin at $$\small b$$, he will end up with a total revenue of $$\small C[b] + S(a,b-1)-R(a,b-1)$$. Since the first player wants to maximize revenue, he chooses the greater of the two, i.e., $$$$$$\small R(a,b) = \text{max}(C[a] + S(a+1,b) - R(a+1,b), C[b] + S(a,b-1) - R(a,b-1))$$.

We can modify the recurrence for $$\small R$$. Since the second player seeks to maximize his revenue, and the total revenue is a constant, it is equivalent for the second player to move so as to minimize the first player's revenue. Therefore, $$\small R(a,b)$$ satisfies the following equations:


$$
R(a,b)=\begin{cases}\text{max}\begin{cases}C[a]+\text{min}\begin{cases}R(a+2,b),\\R(a+1,b-1)\end{cases}\\C[b]+\text{min}\begin{cases}R(a+1,b-1),\\R(a,b-2)\end{cases}\\\end{cases}&\text{if} \,a\le b\\\\0,&\text{otherwise}\end{cases}
$$


The inner $$\small R(a,b)$$ represents the maximum score the second player can obtain playing optimally. Therefore, we want the minimum of that since we're trying to solve for the maximum score the first player can obtain.

```py
def maximum_revenue(coins):

    def compute_maximum_revenue_for_range(a, b):
        if a > b:
            # No coins left
            return 0

        if maximum_revenue_for_range[a][b] == 0:
            maximum_revenue_a = coins[a] + min(compute_maximum_revenue_for_range(a+2, b), 
                                               compute_maximum_revenue_for_range(a+1, b-1))
            maximum_revenue_b = coins[b] + min(compute_maximum_revenue_for_range(a+1, b-1), 
                                               compute_maximum_revenue_for_range(a, b-2))        
            maximum_revenue_for_range[a][b] = max(maximum_revenue_a, maximum_revenue_b)

        return maximum_revenue_for_range[a][b]

    maximum_revenue_for_range = [[0] * len(coins) for _ in range(len(coins))]
    return compute_maximum_revenue_for_range(0, len(coins) - 1)
```

For both solutions, running time and space are bounded by $$\small \mathcal O(n^{2})$$, where $$\small n$$ is the number of coins.

