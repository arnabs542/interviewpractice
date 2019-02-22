#### Generate Strings of Matched Parentheses

> Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.
>
> For example, given n = 3, a solution set is:
>
> ```
> [
>   "((()))",
>   "(()())",
>   "(())()",
>   "()(())",
>   "()()()"
> ]
> ```

##### Recursion:

```py
def generate_balanced_parentheses(num_pairs):
    parens = []

    def helper(left, right, cur):
        if left == right == num_pairs:
            parens.append("".join(cur))
            return
        if right > left or left > num_pairs or right > num_pairs:
            return
        cur.append(')')
        helper(left, right + 1, cur)
        cur.pop()
        cur.append('(')
        helper(left + 1, right, cur)
        cur.pop()

    helper(0, 0, [])
    return parens
```

The brute force solution would to be enumerate all possible $$\small 2^{2n}$$ combinations of the characters '\(' and '\)' and then keep only the valid ones. However, we can use some additional constraints to prune our backtracking a bit earlier. First, the individual number of left parens and right parens must not exceed the given input $$\small n$$. Second, we can only insert a right paren if the number of right parens is currently less than the number of left parens. Otherwise, the string is already invalid.

The time complexity of this problem seems quite difficult to derive. Leetcode simply states that it is the nth Catalan number: 
$$
{\frac{1}{1+n}} \binom{2n}{n} = \frac{4^{n}}{n\sqrt{n}}
$$


