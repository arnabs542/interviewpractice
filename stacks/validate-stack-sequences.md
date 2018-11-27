#### Validate Stack Sequences



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



