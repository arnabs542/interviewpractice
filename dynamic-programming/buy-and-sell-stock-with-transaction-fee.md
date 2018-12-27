#### Buy and Sell Stock with Transaction Fee

> Your are given an array of integers`prices`, for which the`i`-th element is the price of a given stock on day`i`; and a non-negative integer`fee`representing a transaction fee.
>
> You may complete as many transactions as you like, but you need to pay the transaction fee for each transaction. You may not buy more than 1 share of a stock at a time \(ie. you must sell the stock share before you buy again.\)
>
> Return the maximum profit you can make.
>
> **Example 1:**
>
> ```
> Input: prices = [1, 3, 2, 8, 4, 9], fee = 2
>
> Output: 8
>
> Explanation: The maximum profit can be achieved by:
>
>     Buying at prices[0] = 1
>     Selling at prices[3] = 8
>     Buying at prices[4] = 4
>     Selling at prices[5] = 9
>     
> The total profit is ((8 - 1) - 2) + ((9 - 4) - 2) = 8.
> ```

This problem is very similar to **Best Time to Buy and Sell Stock II**, with the addition of a transaction fee. However, the same greedy approach does not work. Let prices = $$\small [1,3,2,8,4,9]$$. If we use the same logic `profit += max(0, prices[i] - prices[i-1] - fee)` to calculate our profit, we would only end up with 7, since our first sell happens at 3. The difference is that now buying/selling a stock costs money, whereas it was free before.

##### Dynamic Programming:

```py
def maxProfit(prices, fee):
    buy, sell = float('-inf'), 0
    for p in prices:
        buy, sell = max(buy, sell - p), max(buy + p - fee, sell)
    return sell
```

The algorithm above relies on two states: `buy`, which is the maximum money we can have after purchasing a stock, and `sell`, which is the maximum money we can have after selling a stock. 

As we iterate through the array, we attempt to maximize both states on each day. If on day $$\small i$$, we are holding a stock, then we are either holding the same share from yesterday \(which itself could've been held from a previous day\), or we bought a share on day $$\small i$$. Thus, `buy = max(buy, sell - prices[i])`.

If we are not holding a share on day $$\small i$$, then we either didn't hold a share yesterday, or we sold today: 

`sell = max(buy + p - fee, buy)`.

Since a buy+sell counts as only one transaction, we can just attach the fee to the sell. 

Running time is $$\small \mathcal O(n)$$. 

