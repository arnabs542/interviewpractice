#### Simplify Path

> Given an absolute pathname that may have `.` or `..` as part of it, return the shortest standardized path.
>
> For example, given "/usr/bin/../bin/./scripts/../", return "/usr/bin/".

##### Stack:

```py
def simplifyPath(path):
    """
    :type path: str
    :rtype: str
    """
    path = path.split("/")
    stack = []
    
    for p in path:
        if not p or p == ".":
            continue
        elif p == "..":
            if stack:
                stack.pop()
        else:
            stack.append(p)
    
    return '/' + "/".join(stack)
```

We simply process the string in the same way we would if doing it by hand. First split the string by `/`, and then go through each element one by one. If the element is a null \(two consecutive `/` characters\) or the element is a `.`, we don't do anything. If the element is `..` we check to see if our stack is empty. If not, pop the last element. For all other elements, simply append to top of stack. In the end, return the elements of the stack joined together with `/`.

Running time is $\small O(n)$, where $\small n$ is the length of the input path.

