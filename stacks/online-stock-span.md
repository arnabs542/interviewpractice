#### Online Stock Span

> Write a class `StockSpanner` which collects daily price quotes for some stock, and returns the _span_ of that stock's price for the current day.
>
> The span of the stock's price today is defined as the maximum number of consecutive days \(starting from today and going backwards\) for which the price of the stock was less than or equal to today's price.
>
> For example, if the price of a stock over the next 7 days were `[100, 80, 60, 70, 60, 75, 85]`, then the stock spans would be `[1, 1, 1, 2, 1, 4, 6]`.
>
> **Example 1:**
>
> ```
> Input: 
> ["StockSpanner","next","next","next","next","next","next","next"]
> , 
> [[],[100],[80],[60],[70],[60],[75],[85]]
> Output: 
> [null,1,1,1,2,1,4,6]
> Explanation: 
>
> First, S = StockSpanner() is initialized.  Then:
> S.next(100) is called and returns 1,
> S.next(80) is called and returns 1,
> S.next(60) is called and returns 1,
> S.next(70) is called and returns 2,
> S.next(60) is called and returns 1,
> S.next(75) is called and returns 4,
> S.next(85) is called and returns 6.
>
> Note that (for example) S.next(75) returned 4, because the last 4 prices
> (including today's price of 75) were less than or equal to today's price.
> ```

##### Code:

```py
class StockSpanner:

    weighted_price = collections.namedtuple("weighted_price", ["price", "weight"])

    def __init__(self):
        self._prices = []


    def next(self, price):
        weight = 1
        while self._prices and price >= self._prices[-1].price:
            weight += self._prices[-1].weight
            self._prices.pop()
        self._prices.append(StockSpanner.weighted_price(price, weight))
        return self._prices[-1].weight
```

##### Explanation:

For each new price entry, we're only concerned about the closest previous element which is strictly greater than it self. The brute force solution would be simply keep an array of the prices, and for each new value, iterate backwards until we find the first element greater than it, if it exists. Obviously, this takes $$\small \mathcal O(n^{2})$$ time, which is not ideal. 

