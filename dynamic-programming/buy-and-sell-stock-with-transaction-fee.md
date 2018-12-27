#### Buy and Sell Stock with Transaction Fee

> Your are given an array of integers`prices`, for which the`i`-th element is the price of a given stock on day`i`; and a non-negative integer`fee`representing a transaction fee.
>
> You may complete as many transactions as you like, but you need to pay the transaction fee for each transaction. You may not buy more than 1 share of a stock at a time \(ie. you must sell the stock share before you buy again.\)
>
> Return the maximum profit you can make.
>
> **Example 1:**  
>
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

Dynamic Programming:

```py
def maxProfit(prices, fee):
    buy, sell = float('-inf'), 0
    for p in prices:
        buy, sell = max(buy, sell - p), max(buy + p - fee, sell)
    return sell
```



