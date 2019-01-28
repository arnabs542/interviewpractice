#### Minimum Cost For Tickets

> In a country popular for train travel, you have planned some train travelling one year in advance.  The days of the year that you will travel is given as an array `days`.  Each day is an integer from `1` to `365`.
>
> Train tickets are sold in 3 different ways:
>
> * a 1-day pass is sold for `costs[0]` dollars;
> * a 7-day pass is sold for `costs[1]` dollars;
> * a 30-day pass is sold for `costs[2]` dollars.
>
> The passes allow that many days of consecutive travel.  For example, if we get a 7-day pass on day 2, then we can travel for 7 days: day 2, 3, 4, 5, 6, 7, and 8.
>
> Return the minimum number of dollars you need to travel every day in the given list of `days`.
>
> **Example 1:**
>
> ```
> Input: days = [1,4,6,7,8,20], costs = [2,7,15]
> Output: 11
> Explanation: 
>
> For example, here is one way to buy passes that lets you travel your travel plan:
> On day 1, you bought a 1-day pass for costs[0] = $2, which covered day 1.
> On day 3, you bought a 7-day pass for costs[1] = $7, which covered days 3, 4, ..., 9.
> On day 20, you bought a 1-day pass for costs[0] = $2, which covered day 20.
> In total you spent $11 and covered all the days of your travel.
> ```
>
> **Example 2:**
>
> ```
> Input: days = [1,2,3,4,5,6,7,8,9,10,30,31], costs = [2,7,15]
> Output: 17
> Explanation: 
>
> For example, here is one way to buy passes that lets you travel your travel plan:
> On day 1, you bought a 30-day pass for costs[2] = $15 which covered days 1, 2, ..., 30.
> On day 31, you bought a 1-day pass for costs[0] = $2 which covered day 31.
> In total you spent $17 and covered all the days of your travel.
> ```
>
> **Note:**
>
> 1. `1 <= days.length <= 365`
> 2. `1 <= days[i] <= 365`
> 3. `days` is in strictly increasing order.
> 4. `costs.length == 3`
> 5. `1 <= costs[i] <= 1000`

##### Brute Force \(DFS\):

```py
def mincostTickets(days: 'List[int]', costs: 'List[int]') -> 'int':

    self.min_cost = float('inf')

    def helper(days, idx, coverage, cost):

        # Base case
        if idx == len(days):
            self.min_cost = min(self.min_cost, cost)
            return

        # Is today covered?
        if days[idx] <= coverage:
            helper(days, idx+1, coverage, cost)

        else:
            # Try all three passes
            helper(days, idx+1, days[idx], cost + costs[0])
            helper(days, idx+1, days[idx] + 6, cost + costs[1])
            helper(days, idx+1, days[idx] + 29, cost + costs[2])

    helper(days, 0, 0, 0)
    return self.min_cost
```

The brute force solution is to simply simulate each day. Our helper function keeps track of which day we're currently on, the max coverage of any previous 7-day or 30-day passes, and the current cost.

On each day, we see if we're covered under a previous pass. If so, then we simply move on to the next day. If not, then we have the choice of purchasing a single, 7-day, or 30-day pass on that particular day. We simulate all three possibilities, then take the cheapest option.

This quickly times out, as each recursion node spawns three children.

##### Dynamic Programming:

```py
def mincostTickets(days: 'List[int]', costs: 'List[int]') -> 'int':

    day_set = set(days)

    @functools.lru_cache(None)
    def dfs(i):
        if i > 365:
            return 0

        if i not in day_set:
            return dfs(i+1)

        return min(costs[0] + dfs(i+1), costs[1] + dfs(i+7), costs[2] + dfs(i + 30))

    return dfs(0)
```

The idea to the above algorithm relies on the same basic principle as the brute force solution.  Let's say `dp(i)` is the cost to fulfill your travel plan from day `i` to the end of the plan. Then, if you have to travel today, your cost is:

$$\text{dp}(i) = \min(\text{dp}(i+1) + \text{costs}[0], \text{dp}(i+7) + \text{costs}[1], \text{dp}(i+30) + \text{costs}[2])$$

Running time is bounded by $$\small O(365)$$, since we're told the maximum day is 365. Space complexity is the same.

##### Bottom-up \(Queue\):

```py
def mincostTickets(days: 'List[int]', costs: 'List[int]') -> 'int':

    last_7, last_30 = collections.deque(), collections.deque()
    cost = 0

    for d in days:
        # Remove expired passes
        while last_7 and last_7[0][0] + 7 <= d:
            last_7.popleft()
        while last_30 and last_30[0][0] + 30 <= d:
            last_30.popleft()
        last_7.append((d, cost + costs[1]))
        last_30.append((d, cost + costs[2]))
        cost = min(cost + costs[0], last_7[0][1], last_30[0][1])

    return cost
```



