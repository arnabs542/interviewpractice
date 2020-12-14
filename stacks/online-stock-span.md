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

It's important to realize that we only need to figure out the span for a price a single time, and we don't need to save of the results. What this means is that we don't need to keep all values, only values relevant to the current price. For example, suppose we get a new element like `7`, and there are some previous elements like `11, 3, 9, 5, 6, 4`. Let's try to create some relationship between this query and the next query.

If \(after getting `7`\) we get an element like `2`, then the answer is `1`. So in general, whenever we get a smaller element, the answer is 1.

If we get an element like `8`, the answer is 1 plus the previous answer \(for `7`\), as the `8` "stops" on the same value that `7` does \(namely, `9`\).

If we get an element like `10`, the answer is 1 plus the previous answer, plus the answer for `9`.

Notice throughout this evaluation, we only care about elements that occur in increasing order - we "shortcut" to them. That is, from adding an element like `10`, we cut to `7` \[with "weight" 4\], then to `9` \[with weight 2\], then cut to `11` \[with weight 1\].

In other words, every time we get a price, we throw out all of the prices directly before it less than or equal to it, and we remember how many we threw out as the "weight". Later prices will either be smaller than it, which we simply append to the stack, or greater than it/equal to it, meaning we can directly reference the weight to avoid having to redo work. 

