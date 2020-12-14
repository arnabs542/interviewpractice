#### Minimum Number of Refueling Stops - LC 871

> A car travels from a starting position to a destination which is target miles east of the starting position.
>
> Along the way, there are gas stations. Each `station[i]` represents a gas station that is `station[i][0]` miles east of the starting position, and has `station[i][1]` liters of gas.
>
> The car starts with an infinite tank of gas, which initially has `startFuel` liters of fuel in it.  It uses `1` liter of gas per `1` mile that it drives.
>
> When the car reaches a gas station, it may stop and refuel, transferring all the gas from the station into the car.
>
> What is the least number of refueling stops the car must make in order to reach its destination?  If it cannot reach the destination, return `-1`.
>
> Note that if the car reaches a gas station with `0` fuel left, the car can still refuel there.  If the car reaches the destination with `0` fuel left, it is still considered to have arrived.
>
> ```
> Example 1:
>
> Input: target = 1, startFuel = 1, stations = []
> Output: 0
> Explanation: We can reach the target without refueling.
> ```
> Example 2:
> ```
> Input: target = 100, startFuel = 1, stations = [[10,100]]
> Output: -1
> Explanation: We can't reach the target (or even the first gas station).
> ```
> Example 3:
> ```
> Input: target = 100, startFuel = 10, stations = [[10,60],[20,30],[30,30],[60,40]]
> Output: 2
> Explanation: 
> We start with 10 liters of fuel.
> We drive to position 10, expending 10 liters of fuel.  We refuel from 0 liters to 60 liters of gas.
> Then, we drive from position 10 to position 60 (expending 50 liters of fuel),
> and refuel from 10 liters to 50 liters of gas.  We then drive to and reach the target.
> We made 2 refueling stops along the way, so we return 2.

##### Solution

The brute force option would be to try all possibilities; at each station we get to, try to get to the destination without refueling there and try to get to the destination with refueling. This leads to some overlap, so we can try to use memoization to reduce the time complexity:

```py
def minRefuelStops(target: int, startFuel: int, stations: "List[List[int]]") -> int:

    # Append target as a final station with no gas
    # Convert distances from absolute position to difference from previous station
    stations += [[target, 0]]
    for i in range(len(stations) - 1, 0, -1):
        stations[i][0] -= stations[i-1][0]    
    n = len(stations)

    @lru_cache(None)
    def helper(cur_fuel, cur_station):
        if cur_station == len(min_refuels):
            return 0

        cur_fuel -= stations[cur_station][0]    # How far we have after reaching stop
        if cur_fuel < 0:    # Not enough
            return float('inf')
        
        return min(1 + helper(cur_fuel + stations[cur_station][1], cur_station + 1), 
                    helper(cur_fuel, cur_station + 1))
    
    refuels = helper(startFuel, 0)
    return refuels if refuels < float('inf') else -1
```
However, this still times out, perhaps due to the overhead of top-down dp. 

The bottom-up solution is as follows:

* Let the dp array `max_distance` keep track of how far we can go with `i` refuels, i.e. `max_distance[0]` is the maximum distance we can reach with `0` refuels, `max_distance[2]` is the maximum we can reach with `2` refuels, etc.
* When we encounter a new station, we iterate backwards through `max_distance`. If `max_distance[j]` could already reach the station with `j` refuels, then we can go up to `max(max_distance[j+1], max_distance[j] + capacity)` by doing one more refuel at the new station.


```py
def minRefuelStops(target: int, startFuel: int, stations: "List[List[int]]") -> int:
    
    n = len(stations)
    max_distance = [startFuel] + [0] * n
    
    for i, (location, capacity) in enumerate(stations):
        for j in range(i, -1, -1):
            if max_distance[j] >= location:
                max_distance[j+1] = max(max_distance[j+1], max_distance[j] + capacity)
    
    for i, distance in enumerate(max_distance):
        if distance >= target:
            return i
    return -1
```

Time complexity is $\small \mathcal O(n^2)$, $\small n$ being the number of stations.

A better solution is to solve this greedily. When we first get to a station (that we can reach), we don't make a decision about whether or not we refuel there, since we could potentially run into a station with more fuel still within our coverage. For example, if we start with `15` fuel, and we have stations `[[5,5], [15,50], ...]`, there's no point at refueling at the first station because the second one is strictly better. 

When we reach a station but have negative fuel (i.e. we needed to have refueled at some point in the past), we will add the capacities of the largest gas stations we've driven by until the fuel is non-negative.

```py
def minRefuelStops(target: int, startFuel: int, stations: "List[List[int]]") -> int:
    
    refuels = 0
    heap = []
    stations.append([target, 0])    # Add the destination as a final station
    
    prev_location = 0
    for location, capacity in stations:
        startFuel -= location - prev_location
        while startFuel < 0:
            if not heap:
                return -1   # Can't reach current station
            startFuel += -heapq.heappop(heap)
            refuels += 1
        heapq.heappush(heap, -capacity)
        prev_location = location
    
    return refuels
```

Running time is now reduced to $\small \mathcal O(n \log{n})$.