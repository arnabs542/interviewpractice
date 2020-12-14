#### Regular Expression Matching - LC 10

> Given an input string \(`s`\) and a pattern \(`p`\), implement regular expression matching with support for `'.'` and `'*'`.
>
> ```
> '.' Matches any single character.
> '*' Matches zero or more of the preceding element.
> ```
>
> The matching should cover the **entire** input string \(not partial\).
>
> **Note:**
>
> * `s` could be empty and contains only lowercase letters `a-z`.
> * `p` could be empty and contains only lowercase letters `a-z`, and characters like `.` or `*`.
>
> **Example 1:**
>
> ```
> Input:
> s = "aa"
> p = "a"
>
> Output: false
> Explanation: "a" does not match the entire string "aa".
> ```
>
> **Example 2:**
>
> ```
> Input:
> s = "aa"
> p = "a*"
>
> Output: true
>
> Explanation: '*' means zero or more of the precedeng element, 'a'. Therefore, by repeating 'a' once, 
> it becomes "aa".
> ```
>
> **Example 3:**
>
> ```
> Input:
> s = "ab"
> p = ".*"
>
> Output: true
>
> Explanation: ".*" means "zero or more (*) of any character (.)".
> ```
>
> **Example 4:**
>
> ```
> Input:
> s = "aab"
> p = "c*a*b"
>
> Output: true
>
> Explanation: c can be repeated 0 times, a can be repeated 1 time. Therefore it matches "aab".
> ```
>
> **Example 5:**
>
> ```
> Input:
> s = "mississippi"
> p = "mis*is*p*."
>
> Output: false
> ```

##### Recursion:

The introduction of the Kleene star makes the problem more difficult. If there was no star, then we simply need to check if the first character in the pattern matches the first character in the text:

```py
def match(text, pattern):
    if not pattern: return not text
    first_match = bool(text) and pattern[0] in {text[0], '.'}
    return first_match and match(text[1:], pattern[1:])
```

With the star, we now have the possibility of the  `_*` pattern in the search pattern.  We may ignore this part of the pattern, since \* allows for zero or matches, or we can delete a matching character from the text. We then move on and see if we can match the rest of the string:

```py
def isMatch(text, pattern):
    if not pattern:
        return not text

    first_match = bool(text) and pattern[0] in {text[0], '.'}

    if len(pattern) >= 2 and pattern[1] == '*':
        return (self.isMatch(text, pattern[2:]) or
                first_match and self.isMatch(text[1:], pattern))
    else:
        return first_match and self.isMatch(text[1:], pattern[1:])
```

##### Dynamic Programming \(Top-down\):

The above solutions will obviously have a lot of overlapping subproblems. Thus, we use dynamic programming to avoid redoing work:

```py
def isMatch(s: str, p: str) -> bool:
    memo = {}

    def helper(i, j):
        # Base conditions
        if (i,j) in memo:
            return memo[(i,j)]
        if j == len(p):
            memo[(i,j)] = (i == len(s))
            return memo[(i,j)]

        first_match = (i < len(s)) and (p[j] in [s[i], '.'])
        if j < len(p) - 1 and p[j+1] == '*':
            memo[(i,j)] = first_match and helper(i+1, j) or helper(i, j+2)
        else:
            memo[(i,j)] = first_match and helper(i+1, j+1)
        return memo[(i,j)]

    return helper(0, 0)
```

In the top-down approach, we use `i,j` to mark the current locations in our string and pattern that we're processing. If we've already processed `s[i:]` and `p[j:]` then we just return whatever result we got. Otherwise, we do the work, store the result, then return it. If $\small m,n$ are the lengths of the string and pattern, then there are a total of $\small \mathcal O(mn)$ combinations of substrings. Thus, our time complexity and space complexity are both bounded by $\small \mathcal O(mn)$.

##### Dynamic Programming \(Bottom-up\):

```py
def isMatch(text, pattern):
    dp = [[False] * (len(pattern) + 1) for _ in range(len(text) + 1)]

    dp[-1][-1] = True
    for i in range(len(text), -1, -1):
        for j in range(len(pattern) - 1, -1, -1):
            first_match = i < len(text) and pattern[j] in {text[i], '.'}
            if j+1 < len(pattern) and pattern[j+1] == '*':
                dp[i][j] = dp[i][j+2] or first_match and dp[i+1][j]
            else:
                dp[i][j] = first_match and dp[i+1][j+1]

    return dp[0][0]
```

This is the same idea as the top-down variant, except we process the two strings backwards. An empty pattern and empty text is a trivial match, so `dp[-1][-1]` is set to True. Otherwise, we check for a star - if there is one, then `dp[i][j] = dp[i+1][j] | (first_match and dp[i][j+2])`. Otherwise, `dp[i][j] = d][i+1][j+1]`. Neither space nor time complexity changes here.

