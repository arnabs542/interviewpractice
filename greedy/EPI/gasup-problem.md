#### The Gasup Problem

> In the gasup problem, a number of cities are arranged on a circular road. You need to visit all the cities and come back to the starting city, A certain amount of gas is avaliable at each city. The mount of gas summed up over all cities is equal to the amount of gas required to go around the road once. Your gas tank has unlimited capacity. Call a city *ample* if you can begin at that city with an empty tank, refill at it, then travel through all the rmaining cities, refilling at each, and return to the ample city, without running out of gass at any point. See the figure below for an example:
> ![](/assets/gasup.png)
> Given an instance of the gasup problem, and given that there eists an ample city, find the city.

##### Solution

The brute force solution is to simply start from every city and see if we can traverse it. This takes $\small \mathcal O(n^{2})$ time. 

The immediate greedy approaches of finding the city with the most gas, smallest distance, or best distance-to-gas ratio don't quite work either. For the given example, $\small A$ has the most gas, but you cannot get to $\small C$. The city $\small G$ has the smallest distance as well as the lowest distance-to-gas ratio, but you cannot reach $\small D$.

The solution itself is actually quite simple - we simply need to find the city that has the lowest amount of gas when we enter it. That city is the ample city.

The proof behind this is a bit more complex. Since we are guaranteed an ample city, then we have the relationship $\small \sum_{i=1}^{n} x_{i} = 0$, where $\small x_{i}$ is $\small gas[i] - cost[i]$. Suppose we reach a point $\small j$ such that:

$\small \sum_{i=1}^{j} x_{i} < 0$

This must mean that:

$\small \sum_{i=j+1}^{n} > 0$ in order to preserve the relationship $\small \sum_{i=1}^{n} x_{i} = 0$.

This also means that if we start at the city right after $\small j$, our trip becomes $\small \sum_{i=j+1}^{n} x_{i} + \sum_{i=1}^{j} x_{i} = 0$, since starting at $\small j+1$ gives us positive gas by the time we start the second leg of the trip.

Another way to look at this problem is visiually:

![](/assets/gasup-graph.png)

The graph of the amount of gas in our tank doesn't change it's shape - it only shifts vertically and horizontally depending on where we start our trip. What this means is that if we start out trip at the city with the lowest amount of gas upon arrival, then the graph is shifted horizontally such that all other cities will have at least 0 gallons in the tank upon arrival. 

```py
# gallons[i] is the amount of gas in city i, and distances[i] is the
# distance city i to the next city.
def find_ample_city(gallons, distances):

    remaining_gallons = 0
    CityAndRemainingGas = collections.namedtuple('CityAndRemainingGas',
                                                 ('city', 'remaining_gallons'))
    city_remaining_gallons_pair = CityAndRemainingGas(0, 0)
    num_cities = len(gallons)
    for i in range(1, num_cities):
        remaining_gallons += gallons[i - 1] - distances[i - 1] // MPG
        if remaining_gallons < city_remaining_gallons_pair.remaining_gallons:
            city_remaining_gallons_pair = CityAndRemainingGas(
                i, remaining_gallons)
    return city_remaining_gallons_pair.city
```

Running time is $\small \mathcal O(n)$.

(https://leetcode.com/problems/gas-station)