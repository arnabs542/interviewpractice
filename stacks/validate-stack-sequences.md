#### Validate Stack Sequences

> Given two sequences `pushed` and `popped` **with distinct values**, return `true` if and only if this could have been the result of a sequence of push and pop operations on an initially empty stack.
>
> **Example 1:**
>
> ```
> Input: pushed = [1,2,3,4,5], popped = [4,5,3,2,1]
> Output: true
> Explanation: We might do the following sequence:
> push(1), push(2), push(3), push(4), pop() -> 4, push(5), pop() -> 5, pop() -> 3, pop() -> 2, pop() -> 1
> ```
>
> **Example 2:**
>
> ```
> Input: pushed = [1,2,3,4,5], popped = [4,3,5,1,2]
> Output: false
> Explanation: 1 cannot be popped before 2.
> ```
>
> **Note:**
>
> 1. `0 <= pushed.length == popped.length <= 1000`
> 2. `0 <= pushed[i], popped[i] < 1000`
> 3. `pushed` is a permutation of `popped`
> 4. `pushed` and `popped` have distinct values.

##### Code \(Simulation\):

```py
def validateStackSequences(pushed, popped):

    stack = []
    pop_idx = 0

    for e in pushed:
        stack.append(e)
        while stack and popped[pop_idx] == stack[-1]:
            stack.pop()
            pop_idx += 1

    return not stack
```

Since we're told that all values are distinct, the sequence can easily be simulated. We keep track of what the next popped item is supposed to be, and we push values until we hit the popped the value. Then we continue to pop according to the popped sequence. In the end, we simply check to make sure the stack is empty. If not, then the pop sequence does not match the push sequence. Runtime and space are both $\small \mathcal O(n)$. 

