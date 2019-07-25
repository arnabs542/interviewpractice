#### Interleave Stack Halves

> Given a stack of N elements, interleave the first half of the stack with the second half reversed using only one other queue. This should be done in-place.
>
> Recall that you can only push or pop from a stack, and enqueue or dequeue from a queue.
>
> **Example 1:**
>
> ```
> Input: [1,2,3,4,5]
> Output: [1,5,2,4,3]
> ```
>
> **Example 2:**
>
> ```
> Input: [1,2,3,4]
> Output: [1,4,2,3]
> ```
>
> Hint: Try working backwards from the end state.

##### Code:

```py
def intervLeave(A):
    if len(A) < 2:
        return A
    queue = collections.deque()

    for i in range(len(A) - 1, -1, -1):
        for j in range(i):
            queue.append(A.pop())
        while queue:
            A.append(queue.popleft())  

    return A
```

Just work backwards and see how to get to the final position. For example, given $\small [1,2,3,4,5]$, we need to get to $\small [1,5,2,4,3]$. To do that we need to go through $\small [1,5,2]$ for the stack, and $\small [4,3]$ for the queue. That requires the stack to have originally been $\small [1,5,2,4,3]$. That in turn requires $\small [1,5]$ for the stack and $\small [2,4,3]$ for the queue, requiring $\small [1,5,3,4,2]$. That requires $\small [1]$ for the stack and $\small [5,3,4,2]$ for the queue, requiring the stack to originally be $\small [1,2,3,4,5]$.

Running time is bounded by $\small \mathcal O(n^{2})$.

