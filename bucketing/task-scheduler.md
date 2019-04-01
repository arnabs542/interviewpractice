#### Task Scheduler

> Given a char array representing tasks CPU need to do. It contains capital letters A to Z where different letters represent different tasks. Tasks could be done without original order. Each task could be done in one interval. For each interval, CPU could finish one task or just be idle.
>
> However, there is a non-negative cooling interval **n** that means between two **same tasks**, there must be at least n intervals that CPU are doing different tasks or just be idle.
>
> You need to return the **least** number of intervals the CPU will take to finish all the given tasks.
>
> **Example:**
>
> ```
> Input: tasks = ["A","A","A","B","B","B"], n = 2
>
> Output: 8
>
> Explanation: A -> B -> idle -> A -> B -> idle -> A -> B.
> ```

##### Bucketing:

```py
def leastInterval(tasks: List[str], n: int) -> int:
    if not n:
        return len(tasks)

    count = collections.Counter(tasks)

    elems_and_counts = [(key, count[key]) for key in count.keys()]
    elems_and_counts.sort(key=lambda x: -x[1])
    
    max_count = elems_and_counts[0][1]

    buckets = [[] for _ in range(max_count)]
    idx = 0

    for e,c in elems_and_counts:
        for i in range(c):
            buckets[idx].append(e)
            idx = (idx + 1) % max(c, max_count - 1)

    time = 0
    for i in range(max_count - 1):
        time += max(len(buckets[i]), n+1)

    time += len(buckets[max_count - 1])
    return time
```

The idea is that we try to pack the tasks into frames. The most frequently appearing elements will each appear in their own frame, then we try to pack the rest of the elements in, trying to get each frame to have $$\small n+1$$ elements. 

Let us use the given example input `tasks = ["A","A","A","B","B","B"], n = 2` to walk through this algorithm. We first group each element and count their occurrences, and then sort them in reversed order based on their count. The array `elems_and_counts` for the given example would appear as such: `[('A', 3), ('B', 3)]`.

The frames will then look like this:

```
A    A    A
B    B    B
```

We don't care about the last frame, since it doesn't need to be filled completely to achieve the condition that same tasks are separated by $$\small n$$ intervals, since there aren't any more tasks. However, for the first two frames, we see that there's only 2 elements in them, when we need three. Thus, each of the frames need an additional idle cycle to fulfill the separation requirement. 

The initial counting takes $$\small \mathcal O(n)$$ time; the sorting takes $$\small \mathcal O(n \log{n})$$ time, and the final bucket construction takes $$\small \mathcal O(n)$$ time. Thus, overall runtime is dominated by the sort, which takes $$\small \mathcal O(n \log{n})$$ time. 

Space usage is $$\small \mathcal O(n)$$. The `counter`, `elems_and_counts` array, and `buckets` array all take $$\small \mathcal O(n)$$ space.

The above solution not only calculates the required time, but actually provides a correct scheduling pattern. We can solve the problem directly by not actually building the bucket, reducing space usage to $$\small \mathcal O(1)$$:

```py
def leastInterval(tasks: "List[str]", n: int) -> int:
    c = collections.Counter(tasks)
    
    max_num = max(c.values())
    t_num = len(c.keys())   
    max_num_t = list(c.values()).count(max_num)
    return max(len(tasks), (n+1) * (max_num - 1) + max_num_t)
```



