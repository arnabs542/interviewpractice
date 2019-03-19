#### The Knapsack Problem

> A thief breaks into a clock store. Each clock has a weight and a value, which are known to the thief. His knapsack cannot hold more than a specified combined weight. His intention is to take clocks whose total value is maximum subject to the knapsack's weight constraint.

##### Brute Force:

```py
def optimum_subject_to_capacity(items, capacity):

    max_value = [0]

    def helper(idx, items, capacity, current_value):
        if idx >= len(items):
            max_value[0] = max(max_value[0], current_value)
            return

        if capacity >= items[idx].weight:
            # Take current item
            helper(idx+1, items, capacity - items[idx].weight, current_value + items[idx].value)
        # Don't take current item
        helper(idx+1, items, capacity, current_value)

    helper(0, items, capacity, 0)
    return max_value[0]
```

The brute force solution is to simply test all possible combinations with a total weight equal to or less than the knapsack weight. Since each item will either be taken or left, our time complexity is bounded by $$\small \mathcal O(2^{n})$$, which will quickly blow up for larger inputs.

##### Dynamic Programming \(Incorrect\):

```py
def optimum_subject_to_capacity(items, capacity):

    dp = [[0] * len(items) for _ in range(capacity + 1)]

    for i in range(1, capacity + 1):
        for j in range(len(items)):
            if items[j].weight <= i:
                dp[i][j] = max(dp[i][j-1], dp[i - items[j].weight][j-1] + items[j].value)
            else:
                dp[i][j] = dp[i][j-1]

    return dp[-1][-1]
```

Most recursive problems can be optimized with dynamic programming. The idea of the above is solution is this: let dp be the dynamic programming memo that we use. The entry at `dp[i][j]` represents the maximum value the thief can achieve with a knapsack of weight `i`, and he only has the first `j` items to pick from. If `item[j].weight <= i`, then the thief can choose to take the item, meaning that the maximum value he can obtain after is `dp[i - item[j].weight][j-1]`. If he chooses to not take the item, then he can achieve a current max value of `dp[i][j-1]`\(the previous maximum value he achieved without being able to access the current item\).

However, this approach is not correct because we will end up counting duplicates. For instance, suppose we're currently dealing with weight `k > i`. When we consider `items[j]`, if we choose to include it, we will set `dp[k][j] = items[j].value + dp[k-items[j].weight][j-1]`. However, the value at `dp[k-items[j].weight][j-1]` could also contain `items[j]`, leading to incorrectly duplicate counting.

##### Dynamic Programming:

```py
def optimum_subject_to_capacity(items, capacity):

    dp = [[0] * (capacity + 1) for _ in range(len(items))]

    for i in range(len(items)):
        for j in range(1, capacity+1):
            if items[i].weight <= j:
                dp[i][j] = max(dp[i-1][j], dp[i-1][j - items[i].weight] + items[i].value)
            else:
                dp[i][j] = dp[i-1][j]

    return dp[-1][-1]
```

To avoid counting duplicates, we need to simply switch the ordering of the array. Instead of increasing the weight in the outer loop, we instead introduce each item. This way, we guarantee that all previous calculated values will not have the current item factored in - thus eliminating duplicates.

The running time and space complexity are bounded by $$\small \mathcal O(n*w)$$, where $$\small n,w$$ represent the number of items and the total capacity of the knapsack.

We can also reduce space complexity to $$\small \mathcal O(w)$$ by only keep the two relevant rows. Time complexity doesn't change.

```py
def optimum_subject_to_capacity(items, capacity):

    dp = [[0] * (capacity + 1) for _ in range(2)]
    prev_row, cur_row = 0, 1

    for i in range(len(items)):
        for j in range(capacity + 1):
            if items[i].weight <= j:
                dp[cur_row][j] = max(dp[cur_row][j], dp[prev_row][j - items[i].weight] + items[i].value)
            dp[cur_row][j] = max(dp[cur_row][j], dp[prev_row][j])
        prev_row, cur_row = cur_row, prev_row
    return dp[prev_row][-1]
```



