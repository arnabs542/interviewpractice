#### Render a Calendar

> Consider the problem of designing an online calendaring application. One component of the design is to render the calendar, i.e., display it visually.
>
> Suppose each day consists of a number of events, where an event is specified as a start time and a finish time. Individual events for a day are to be rendered as nonoverlapping rectangular regions whose sides are parallel to the $\small X-$ and $\small Y-$axes. Let the $\small X$-axis correspond to time. If an event starts at time $\small b$ and ends at time $\small e$, the upper and lower sides of its corresponding rectangle must be at $\small b$ and $\small e$, respectively.
>
> Write a program that takes a set of events and determines the maximum number of events that take place concurrently.

##### Code:

```py
def find_max_simultaneous_events(A):
    starts, ends = [a.start for a in A], [a.finish for a in A]
    starts.sort(), ends.sort()
    i = 0
    count = 0
    for s in starts:
        if s <= ends[i]:
            count += 1
        else:
            i += 1
    return count
```

##### Explanation:

Each event can be thought as two distinct events: a start and an end. We don't need more space unless we have a start occurring before an end. This realization is the key to the algorithm above. We simply decompose all events into starting and ending times. We iterate through the starting array, and for each start that occurs before the end event we're waiting on, we need an additional room. Otherwise, we increment the end point forward to the next end event.

I'm not completely sure why we don't need to decrement count when s &gt; ends\[i\]. To be completely safe, we can simply use two indices, one to traverse through each array. That is basically what the book solution does:

```py
def find_max_simultaneous_events(A):

    # Builds an array of all endpoints.
    E = [
        p for event in A for p in (Endpoint(event.start, True),
                                   Endpoint(event.finish, False))
    ]
    # Sorts the endpoint array according to the time, breaking ties by putting
    # start times before end times.
    E.sort(key=lambda e: (e.time, not e.is_start))

    # Track the number of simultaneous events, record the maximum number of
    # simultaneous events.
    max_num_simultaneous_events, num_simultaneous_events = 0, 0
    for e in E:
        if e.is_start:
            num_simultaneous_events += 1
            max_num_simultaneous_events = max(num_simultaneous_events,
                                              max_num_simultaneous_events)
        else:
            num_simultaneous_events -= 1
    return max_num_simultaneous_events
```

Running time is $\small \mathcal O(n \log{n})$, which means our running time is dominated by the initial sort we perform.

