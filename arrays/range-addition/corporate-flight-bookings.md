#### Corporate Flight Bookings

> There are n flights, and they are labeled from 1 to n.
>
> We have a list of flight bookings. The `i`-th booking `bookings[i] = [i, j, k]` means that we booked `k` seats from flights labeled `i` to `j` inclusive.
>
> Return an array answer of length `n`, representing the number of seats booked on each flight in order of their label.
>
>Example 1:
> ```
> Input: bookings = [[1,2,10],[2,3,20],[2,5,25]], n = 5
> Output: [10,55,45,25,25]
> ```
> Constraints:
>
> * 1 <= `bookings.length` <= 20000
> * 1 <= `bookings[i][0]` <= `bookings[i][1]` <= `n` <= 20000
> * 1 <= `bookings[i][2]` <= 10000

##### Solution

Quite literally the same problem as [Range Addition](arrays/range-addition/range-addition.md).

```py
def corpFlightBookings(bookings: "List[List[int]]", n: int) -> "List[int]":
    flights = [0] * n
    for i, j, k in bookings:
        flights[i-1] += k
        if j < n:
            flights[j] -= k
    
    for i in range(1, n):
        flights[i] += flights[i-1]
    
    return flights
```

##### Notes

In future problems that ask for updating values with ranges, consider using this prefix-sum approach.