#### Most Occupants

> You are given a list of data entries that represent entries and exits of groups of people into a building. An entry looks like this:
>
> `{"timestamp": 1526579928, count: 3, "type": "enter"}`
>
> This means 3 people entered the building. An exit looks like this:
>
> `{"timestamp": 1526580382, count: 2, "type": "exit"}`
>
> This means that 2 people exited the building. `timestamp` is in [Unix time](https://en.wikipedia.org/wiki/Unix_time).
>
> Find the busiest period in the building, that is, the time with the most people in the building. Return it as a pair of `(start, end)` timestamps. You can assume the building always starts off and ends up empty, i.e. with 0 people inside.

##### Sorting + Kadane's algorithm:

```py
def maxOccupants(arr):

    max_occupants = float('-inf')
    start = end = cur_occupants = 0
    time = (-1, -1)

    for i,e in enumerate(arr):
        if e.type == enter:
            if e.count + cur_occupants >= cur_occupants:
                start = i
            cur_occupants += e.count
        else:
            cur_occupants -= e.count
        if cur_occupants > max_occupants:
            time = (start, i)
    
    return time
```

This problem is basically a combination of the Render a Calendar problem with Maximum Subarray. There are a few assumptions made in the solution:

1. The array is sorted by timestamp. If not, we will need to do so, breaking ties by placing enters before exits. 
2. The array is valid, i.e. we won't have an exit as the first entry, more exits then entries, etc. 

We iterate through the array, and whenever we encounter an entry incident, we add the count to our running sum, and whenever we encounter an exit incident, we subtract its count. We update the start when the current enter count is greater than/equal to if we had continued counting from the current starting period. However, if we assume that array is valid, all this does is keep the time frame to the shortest period which has a number of people equal to the largest count. This is because we're assuming we never have a negative amount of people in the building.

