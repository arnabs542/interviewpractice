#### Coin Change

> You are given coins of different denominations and a total amount of money amount. Write a function to compute the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return `-1`.
>
> **Example 1:**
>
> ```
> Input: coins = [1, 2, 5], amount = 11
> Output: 3
> Explanation: 11 = 5 + 5 + 1
> ```
>
> **Example 2:**
>
> ```
> Input: coins = [2], amount = 3
> Output: -1
> ```

##### DP \(Bottom up\):

```py
def coinChange(coins: "List[int]", amount: "int") -> "int":
    count = [0] + [float('inf')] * (amount)
    coins = set(coins)
    for i in range(amount+1):
        if i in coins:
            count[i] = 1
        else:
            for c in coins:
                if c > i:
                    continue
                count[i] = min(count[i], count[i - c] + 1)

    return count[-1] if count[-1] < float('inf') else -1
```

The idea of the solution is simple: if we want to get to some amount $$\small n$$ with a set of coins $$\small c$$, we need to find out how much it takes to get to the amounts$$\small \{n - c_{i} \, \text{for} \, c_{i} \, \text{in} \, c\}$$.

