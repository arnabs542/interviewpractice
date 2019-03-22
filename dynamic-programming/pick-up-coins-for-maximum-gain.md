#### Pick Up Coins for Maximum Gain

> In the pick-up-coins game, and even number of coins are placed in a line. Two players take turns at choosing one coin each - they can only choose from the two coins at the ends of the line. The game ends when all the coins have been picked up. The player whose coins have the higher total value wins. A player cannot pass his turn.
>
> Design an efficient algorithm for computing the maximum total value for the starting player in the pick-up-coins game.

##### Dynamic Programming

It's important to note that a greedy approach will not work for this problem. Suppose the coins are \[5, 25, 10, 1\]. If the first player chooses 5, the second player will choose 25, and thus win. The first player needs to choose 1 in order to force the second player to expose the 25, and thus lose the game. This leads to a key insight - we can reframe the problem as maximizing the difference between the two final scores.

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



