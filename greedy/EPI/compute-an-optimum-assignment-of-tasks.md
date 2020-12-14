#### Compute an Optimum Assignment of Tasks

> Consider the problem of assigning tasks to workers. Each worker must be assigned exactly two tasks. Each task takes a fixed amount of time. Tasks are independent, i.e., there are no constraints of the form "Task 4 cannot start before Task 3 is completed." Any task can be assigned to any worker. 
>
> We want to assign tasks to workers so as to minimize how long it takes before all tasks are completed. For example, if there are 6 tasks whose durations are 5,2,1,6,4,4 hours, then an optimum assignment is to give the first two tasks \(i.e., the tasks with durations 5 and 2\) to one worker, the next two \(1 and 6\) to another worker, and the last two tasks \(4 and 4\) to the last worker. For this assignment, all tasks will finish after\(max\(5+2, 1+6, 4+4\) = 8 hours.

##### Sort + Average:

```
def optimum_task_assignment(task_durations):
    task_durations.sort()
    i, j = 0, len(task_durations) - 1
    pairing = []
    while i < j:
        pairing.append((task_durations[i], task_durations[j]))
        i += 1
        j -= 1
    return pairing
```

Since we want to minimize the time the longest pair will take, it makes sense to pair the tasks towards the average. In other words, if we give a worker a long task, we would want to try to minimize his other task.

Algorithm takes $\small \mathcal O(n \log{n})$ time to run due to sorting.  

