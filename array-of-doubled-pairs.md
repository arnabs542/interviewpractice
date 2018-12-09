#### Array of Doubled Pairs

> Given an array of integers `A` with even length, return `true` if and only if it is possible to reorder it such that `A[2 * i + 1] = 2 * A[2 * i]` for every `0 <= i < len(A) / 2`.
>
> **Example 1:**
>
> ```
> Input: [3,1,3,6]
> Output: false
> ```
>
> **Example 2:**
>
> ```
> Input: [2,1,2,6]
> Output: false
> ```
>
> **Example 3:**
>
> ```
> Input: [4,-2,2,-4]
> Output: true
> Explanation: We can take two groups, [-2,-4] and [2,4] to form [-2,-4,2,4] or [2,4,-2,-4].
> ```
>
> **Example 4:**
>
> ```
> Input: [1,2,4,16,8,4]
> Output: false
> ```

##### Hash Table \(Element Pairing\):

```py
def canReorderDoubled(A):
    c = collections.Counter(A)
    for x in sorted(c, key=abs):
        if c[x] > c[2 * x]:
            return False
        c[2 * x] -= c[x]
    return True
```

The pattern for the array is this: `A[1] = 2*A[0], A[3] = 2*A[2]`, etc. Since there is no restriction about where elements can be placed, we can break the array down into a collection of pairs, and we only need to consider each pair at a time. For example, if we 4 2's in the array, then we need at least 4 1's or 4 4's or 2 2's and 2 1's. We can then start from the smallest number $$\small x$$ in each pair and look for $$\small 2*x$$. If we can't find any, then we simply return False.

##### Edge Cases:

When I first did the problem, I didn't realize negative numbers would present a problem, since if we sort without specifying absolute value, and array of $$\small [-2, -4, 4, 2]$$ would end up as $$\small [-4, -2, 2, 4]$$. Then when we iterate through it, we would end up with $$\small -4 * 2 = -8$$, which is not present in the array, causing us to return a 0.

It's also important to note the difference between `x//y` and `int(x/y)`. The first operation rounds down; for the case of `-3//2`, the answer would be `-2`. The second operation simply truncates the floating portion, and `int(-3/2) = -1`.

