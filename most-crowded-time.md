#### Most Crowded Time

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
def maxOccupants(entries):
    cur_occupants = 0
    max_occupants = float('-inf')
    time = (-1, -1)

    start = end = 0

    for i,e in enumerate(entries):
        if e + cur_occupants < e:
            cur_occupants = 0
            start = i
        cur_occupants += e
        if cur_occupants > max_occupants:
            max_occupants = cur_occupants
            max_index = (start, i)

    return max_occupants
```

This problem is basically a combination of the Render a Calendar problem with Maximum Subarray. There are a few assumptions made in the solution:

1. The array 



