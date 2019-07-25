#### Score of Parentheses

> Given a balanced parentheses string S, compute the score of the string based on the following rule:
>
> * () has score 1
> * AB has score A + B, where A and B are balanced parentheses strings.
> * (A) has score 2 * A, where A is a balanced parentheses string.
> 
> Example 1:
> ```
> Input: "()"
> Output: 1
> ```
> Example 2:
> ```
> Input: "(())"
> Output: 2
> ```
> Example 3:
> ```
> Input: "()()"
> Output: 2
>
> Example 4:
> 
> Input: "(()(()))"
> Output: 6
>```

##### Solution

The brute force solution is to split the string into `S = A + B` where `A` and `B` are balanced parentheses strings, and `A` is the smallest possible non-empty prefix of `S`.

Call a balanced string _primitive_ if it cannot be partitioned into two non-empty balanced strings.

By keeping track of balance (the number of `(` parentheses minus the number of `)` parentheses), we can partition `S` into primitive substrings `S = P_1 + P_2 + ... + P_n`. Then, `score(S) = score(P_1) + score(P_2) + ... + score(P_n)`, by definition.

For each primitive substring (`S[i]`, `S[i+1]`, ..., `S[k]`), if the string is length `2`, then the score of this string is `1`. Otherwise, it's twice the score of the substring (`S[i+1]`, `S[i+2]`, ..., `S[k-1]`).

```py
def scoreOfParentheses(S: str) -> int:
    def F(i, j):
        # Returns score of S[i:j]
        ans = bal = 0
        
        # Try to split strings into primatives
        for k in range(i, j):
            bal += 1 if S[k] == '(' else -1
            if bal == 0:
                if k == i + 1:
                    ans += 1
                else:
                    ans += 2 * F(i+1, k)
                i = k + 1
        return ans
    
    return F(0, len(S))
```

The running time for this approach is $\small \mathcal O(n^{2})$. This can be seen in the worst case scenario of `((((...))))`. Each level we remove only 2 characters, leading to a recurrence relationship of $\small T(n) = O(n) + T(n-2)$.

However, we can solve this with a single pass through the string. Whenever we encounter a `(`, we essentially enter a new level. When we encounter a `)`, we multiple the previous level by 2, unless it's a `()`, in which case we simply add `1` to the score. 

```py
def scoreOfParentheses(S: str) -> int:
    stack = [0] # The very bottom of the stack hold overall score
    for c in S:
        if c == '(':    # New level
            stack.append(0)
        else:
            # We've encountered ')' - level is finished. Calculate score
            prev_score = stack.pop()
            stack[-1] += max(prev_score * 2, 1)
    return stack[0]
```
