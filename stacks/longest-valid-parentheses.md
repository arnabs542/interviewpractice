#### Longest Valid Parentheses

> Given a string containing just the characters `'('` and `')'`, find the length of the longest valid \(well-formed\) parentheses substring.
>
> **Example 1:**
>
> ```
> Input: "(()"
>
> Output: 2
>
> Explanation: The longest valid parentheses substring is "()"
> ```
>
> **Example 2:**
>
> ```
> Input: ")()())"
>
> Output: 4
>
> Explanation: The longest valid parentheses substring is "()()"
> ```

##### Brute Force:

```py
def longestValidParentheses(s):
    """
    :type s: str
    :rtype: int
    """
    if '(' not in s or ')' not in s:
        return 0
    max_length = 0
    for i in range(len(s)):
        left = 0
        for j in range(i, len(s)):
            if s[j] == ')':
                if left <= 0:
                    max_length = max(max_length, j - i)
                    break
                else:
                    left -= 1
                    if left == 0:
                        max_length = max(max_length, j - i + 1)
            else:
                left += 1     
    return max_length
```

The brute force solution would be to simply iterate through each character, and try to build as long a valid sequence as possible. We perform the validity check as we iterate, so it saves a bit of time. The variable `left` keeps track of how many $$\small ($$ brackets we have in the current sequence. We advance until the end of the string or we encounter more $$\small )$$ brackets than their counterparts. Be careful of ending indices - if we encounter an invalid character at $$\small s[j]$$, we calculate the length by $$\small j-i$$, since $$\small j$$ should not be included. If the character at $$\small s[j]$$ validates the array, we need to add one to our calculation, since $$\small j$$ is included in the sequence. 

The runtime is bounded by $$\small \mathcal O(n^{2})$$.

##### Stack:



