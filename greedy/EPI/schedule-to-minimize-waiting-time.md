#### Schedule to Minimize Waiting Time

> A database has to respond to a set of client SQL queries. The service time required for each query is known in advance. For this application, the queries must be processed by the database one at a time, but can be done in any order. The time a query waits before its turn comes is called its waiting time.
>
> Given service times for a set of queries, compute a schedule for processing the queries that minimizes the total waiting time. Return the minimum waiting time. For example, if the service times are $$\small <2,5,1,3>$$, if we schedule in the given order, the total waiting time is $$\small 0 + (2) + (2+5) + (2+5+1) = 17$$. If however, we schedule queries in order of decreasing service times, the total waiting time is $$\small 0 + (5) + (5+3) + (5+3+2)=23$$. For this example, the minimum waiting time is 10.

##### Sort + Running Sum:

```py
def minimum_total_waiting_time(service_times):
    if not service_times:
        return 0

    service_times.sort()
    prev_waiting_time, total_waiting_time = service_times[0], 0
    for i in range(1, len(service_times)):
        total_waiting_time += prev_waiting_time
        prev_waiting_time += service_times[i]
        
    return total_waiting_time
```

It's important to first figure out what the question is asking. The total amount of time to finish the tasks doesn't change - what we're concerned about is minimizing the total amount of time each task needs to wait before being executed. 

The wait time for a single task is summation of the service time of all tasks scheduled ahead of it. Therefore, for a list of $$\small n$$ queries, the service time of the first scheduled task will be added $$\small n-1$$ times, the service time of the second scheduled task will be added $$\small n-2$$ times, etc. Thus, it becomes clear that the optimal schedule is achieved by simply sorting the service times.

Running time is $$\small \mathcal O(n \log{n})$$.

