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

```py
def longestValidParentheses(s):
    """
    :type s: str
    :rtype: int
    """
    stack = []
    max_length = 0
    for i,c in enumerate(s):
        if c == '(':
            stack.append(i)
        else:
            if not stack or s[stack[-1]] == ')':
                stack.append(i)
            else:
                stack.pop()
                max_length = max(max_length, i - (-1 if not stack else stack[-1]))

    return max_length
```

The idea to this problem is pretty similar to the much easier problem of validating a sequence of parentheses. When we check for  a valid sequence, we push each opening bracket onto the stack and pop when we see a closing bracket, assuming the stack isn't empty. We follow the same procedure here, except we must also remember where the current substring began. We do so by pushing the invalidating character's position into the stack.

Let the input be $$\small ) \, ( \, )\, (\, )\, )$$. We begin with an empty stack. The first character is $$\small ($$, and since there is no opening bracket, we push 0 onto the stack. This represents that the next valid substring will being at index 1 at the earliest. If we do not push 0 onto the stack, then the substring $$\small ()()$$ will only render as 2 substrings of size 2, instead of 1 substring of size 4.

Running time and space are both bounded by $$\small \mathcal O(n)$$.

##### Dynamic Programming:

```py
def longestValidParentheses(s):
    """
    :type s: str
    :rtype: int
    """
    dp = [0] * len(s)
    max_length = 0

    for i in range(1, len(s)):
        if s[i] == ')':
            if s[i-1] == '(':
                dp[i] = 2 if i-2 < 0 else dp[i-2] + 2
            elif i - dp[i-1] - 1 >= 0 and s[i - dp[i-1] - 1] == '(':
                dp [i] = dp[i-1] + (dp[i - dp[i-1] - 2] if i - dp[i - 1] >= 2 else 0) + 2
            max_length = max(max_length, dp[i])

    return max_length
```

We make use of dp array where $$\small dp[i]$$ represents the length of the longest valid substring ending at index $$\small i$$. It's obvious that the valid substrings must end with $$\small ‘‘\,)"$$. This further leads to the conclusion that the substrings ending with $$\small ‘‘(\,"$$ will always contain 0 at their corresponding dp indices. Thus, we update the dp array only when $$ \small ‘‘\,)"$$ is encountered.

To fill the dp array we will check every two consecutive characters of the string:



To filldp\text{dp}dp array we will check every two consecutive characters of the string and if

1. $$\small s[i] = \, ‘‘\, )"$$ and $$\small s[i-1] = \,‘‘(\,"$$, i.e. string looks like $$\small ‘‘.....()"$$ $$\small ⇒ dp[i] = dp[i-1] + 2$$

   In this case, the ending $$\small ‘‘\,()"$$ portion is a valid substring by itself and leads to an increment of 2 in the length of the just previous valid substring's length.

2. $$\small s[i] = \, ‘‘\, )"$$ and $$\small s[i-1] = \,‘‘\,)"$$, i.e. string looks like $$\small ‘‘.....))"$$ ⇒

   if $$\small s[i−dp[i−1]−1]=‘‘(\,"$$ then $$\small dp[i] = dp[i-1] + dp[i-dp[i-1]-2]+2$$ $$\\$$ 

   The reason behind this is that if the 2nd last‘\)’\text{‘\)’}‘\)’ was a part of a valid substring \(saysubssub\_ssubs​\), for the last‘\)’\text{‘\)’}‘\)’ to be a part of a larger substring, there must be a corresponding starting‘\(’\text{‘\(’}‘\(’ which lies before the valid substring of which the 2nd last‘\)’\text{‘\)’}‘\)’ is a part \(i.e. beforesubssub\_ssubs​\). Thus, if the character beforesubssub\_ssubs​ happens to be‘\(’\text{‘\(’}‘\(’, we update thedp\[i\]\text{dp}\[i\]dp\[i\] as an addition of222 in the length ofsubssub\_ssubs​ which isdp\[i−1\]\text{dp}\[i-1\]dp\[i−1\]. To this, we also add the length of the valid substring just before the term "\(,sub\_s,\)" , i.e.dp\[i−dp\[i−1\]−2\]\text{dp}\[i-\text{dp}\[i-1\]-2\]dp\[i−dp\[i−1\]−2\].





