#### Valid Parenthesis String

> Given a string containing only three types of characters: `(`, `)` and `*`, write a function to check whether this string is valid. We define the validity of a string by these rules:
>
> * Any left parenthesis `(` must have a corresponding right parenthesis `)`.
> * Any right parenthesis `)` must have a corresponding left parenthesis `(`.
> * Left parenthesis `(` must go before the corresponding right parenthesis `)`.
> * `*` could be treated as a single right parenthesis `)` or a single left parenthesis `(` or an empty string.
> * An empty string is also valid.
>
> Example 1:
> ```
> Input: "()"
> Output: True
> ```
> Example 2:
> ```
> Input: "(*)"
> Output: True
> ```
> Example 3:
> ```
> Input: "(*))"
> Output: True
> ```

##### Solution

The brute force solution is to assign every `*` to both possibilities and then test is the resulting string is valid. This takes $\small \mathcal O(n * 2^{m})$ time, where $\small m$ is the number of `*` characters. Obviously this is very costly, and we will avoid having to do so. 

My initial approach was to approach this the same way as the simpler problem of simply verifying a valid parenthesis string. I used `left`, `right`, and `stars` to keep track of the number of `(`, `)`, and `*`. However, the problem with this approach is that in a string like `**(()*`, the `)` cannot use the star behind it, while the `((` cannot use the two stars ahead of it. Therefore, simply by counting the number of characters will return `True`, even though this string is actually not valid. 

The trick is instead to reframe the problem. What this problem is asking is at the end of the string, how many `)` are we waiting for still? If we don't need any more closing parenthesis, then we have a valid string. If at any point, we encounter too many closing parenthesis, then return `False` directly. We keep two variables: `min_open` and `max_open` that keep track of the minimum number of open parenthesis and max umber of open parenthesis. 

Take the string `(*(` as an example. When we meet the first `(`, we know we need exactly one `)`. Therefore, `min_open = max_open = 1`. When we meet the `*`, we can assign that either as an empty string, a `(`, or a `)`. However, we know now that from the rest of the string, the minimum number of `)` we need has gone down by one, since we can use the star. Thus, we can decrement `min_open`. However, we must take care not to let it become negative, since that would signify we assigned too many `)`. 

In summary, we count the number of `)` we are waiting for, and if it's equal to the number of open parenthesis. This number will be a range and tracked by `min_open, max_open`. 

The variable `max_open` counts the maximum open parenthesis,
which means the maximum number of unbalanced `(` that **COULD** be paired, while `min_open` counts the minimum open parenthesis, which means the number of unbalanced `(` that **MUST** be paired.

```py
def checkValidString(s: str) -> bool:
    min_open, max_open = 0, 0
    for c in s:
        if c == '(':
            min_open += 1
            max_open += 1
        elif c == ')':
            min_open = max(0, min_open - 1)
            max_open -= 1
        else:
            min_open = max(0, min_open - 1)
            max_open += 1
        if max_open < 0:
            return False
    return min_open == 0
```

Running time is $\small \mathcal O(n)$.