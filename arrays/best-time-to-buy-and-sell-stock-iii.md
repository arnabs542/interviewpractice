#### Best Time to Buy and Sell Stock III

Say you have an array for which the _i\_th element is the price of a given stock on day \_i_.

Design an algorithm to find the maximum profit. You may complete at most _two_ transactions.

**Note: **You may not engage in multiple transactions at the same time \(i.e., you must sell the stock before you buy again\).

**Example 1:**

```
Input: [3,3,5,0,0,3,1,4]
Output: 6
Explanation: Buy on day 4 (price = 0) and sell on day 6 (price = 3), profit = 3-0 = 3.
             Then buy on day 7 (price = 1) and sell on day 8 (price = 4), profit = 4-1 = 3.
```

**Example 2:**

```
Input: [1,2,3,4,5]
Output: 4
Explanation: Buy on day 1 (price = 1) and sell on day 5 (price = 5), profit = 5-1 = 4.
             Note that you cannot buy on day 1, buy on day 2 and sell them later, as you are
             engaging multiple transactions at the same time. You must sell before buying again.
```

**Example 3:**

```
Input: [7,6,4,3,1]
Output: 0
Explanation: In this case, no transaction is done, i.e. max profit = 0.
```

##### Brute Force:

```py
def find_profit(prices):
    if not prices:
        return 0
    max_profit, min_price = 0, prices[0]
    for i in range(len(prices)):
        min_price = min(min_price, prices[0])
        max_profit = max(max_profit, prices[i] - min_price)
    return max_profit

def buy_and_sell_stock_twice(prices):
    # Iterate through array and find max and min for each division
    max_profit = 0
    for i in range(len(prices)):
        max_profit = max(max_profit, find_profit(prices[:i]) + find_profit(prices[i:]))
    return max_profit
```

Since we're not allowed to hold stocks while buying, the array is essentially split into two halves: the first half is where we make our first transaction, and the second half where we make our second transaction. Thus, we can iterate through the array, splitting it into two parts each time, and then find the maximum profit from both half.

This takes $$\small \mathcal O(n^{2})$$.

##### Two-pass:

```py
def buy_and_sell_stock_twice(prices):
    max_total_profit, min_price_so_far = 0.0, float('inf')
    first_buy_sell_profits = [0] * len(prices)
    # Forward phase
    for i, price in enumerate(prices):
        min_price_so_far = min(min_price_so_far)
        max_total_profit = max(max_total_profit, price - min_price_so_far)
        first_buy_sell_profits = max_total_profit
    # Back phase
    max_price_so_far = float('-inf')
    for i in reversed(list(enumerate(prices[1:], 1))):
        max_price_so_far = max(max_price_so_far, price)
        max_total_profit = max(max_total_profit, max_price_so_far - price + first_buy_sell_profits[i-1])
    return max_total_profit
```

We can solve the problem in $$\small \mathcal O(n)$$ for both time and space. We first iterate forwards, building a vector of the max profit if we sell buy date `i`. We then iterate backwards, and find the max profit working from the back. This way, we've essentially split the array without having to do extra work.

Suppose the input array is: $$\small <12, 11, 13, 9, 12, 8, 14, 13, 15>$$.

Forward profit is: $$\small< 0, 0, 2, 2, 3, 3, 6, 6,7>$$

Backward profit is: $$\small < 7, 7, 7, 7, 7, 7, 2, 2, 0>$$

The maximum profit is: $$\small< 7, 7, 7, 9, 9, 10, 5, 8, 6>$$, i.e. the max profit is 10.

The forward pass is basically saying what's the max profit if we buy and sell by date `i`. The backward pass is saying the same thing, except backwards. By find the max\_profit from the backward pass and adding that to the max\_profit from forward pass at `i-1`, we're basically saying what's the max pass if we split the array at `i`.

