#### Regular Expression Matching

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
> * `s` could be empty and contains only lowercase letters `a-z`.
> * `p` could be empty and contains only lowercase letters `a-z`, and characters like `.` or `*`.
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
> Explanation: '*' means zero or more of the precedeng element, 'a'. Therefore, by repeating 'a' once, 
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
> Explanation: ".*" means "zero or more (*) of any character (.)".
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
> Explanation: c can be repeated 0 times, a can be repeated 1 time. Therefore it matches "aab".
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

##### Dynamic Programming:



