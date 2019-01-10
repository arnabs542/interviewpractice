#### Find First Occurrence of Substring

> Given a string and a pattern, find the starting indices of all occurrences of the pattern in the string. For example, given the string "abracadabra" and the pattern "abr", you should return `[0, 7]`.

The problem prompt from above is from a DCP question asked by Microsoft. I will solve the simpler version of finding the first occurrence of the pattern as a way of introducing the Rabin-Karp and Knuth-Morris-Pratt algorithms commonly used for string matching.

##### Brute Force:

```py
def findPattern(s, p):
    """
    s: string
    p: pattern
    """
    if len(p) > len(s):
        return -1
    for i in range(len(s)):
        if s[i: i + len(p)] == p:
            return i
        if len(s) - i < len(p):
            return -1
    return -1
```

The simple way of solving the problem would be compare every substring with a length of the pattern against the pattern itself. If it matches, we're done. Runtime is bounded by $$\small \mathcal O(s*p)$$, where $$\small s,p$$ represent the lengths of the string and the pattern. For each comparison, we need to iterate through all characters in the substring. We will need to do this $$\small s - p$$ times, which can be approximated by $$\small s$$ in the event $$\small s$$ is much longer than $$\small p$$.

##### Rabin-Karp:

```py
def rabin_karp(t, s):
    if len(s) > len(t):
        return -1

    BASE = 26
    t_hash = functools.reduce(lambda h, c: h * BASE + ord(c), t[:len(s)], 0)
    s_hash = functools.reduce(lambda h, c: h * BASE + ord(c), s, 0)
    power_s = BASE**max(len(s) - 1, 0)

    for i in range(len(s), len(t)):
        if t_hash == s_hash and t[i-len(s):i] == s:
            return i - len(s)

        # Use rolling hash to update hash code
        t_hash -= ord(t[i - len(s)]) * power_s
        t_hash = t_hash * BASE + ord(t[i])

    # Tries to match s and t[-len(s):]
    if t_hash == s_hash and t[-len(s):] == s:
        return len(t) - len(s)

    return -1
```

The Rabin-Karp algorithm relies on hashing to reduce the amount of checks. If the current substring isn't hash-equivalent to the pattern, then we automatically move on without checking. 

The hashing provided above is simply  a base change. We take the ASCII value of the character at each position, and multiply it by 26 to the power for the position. For example, a string of `'abc'` would result in a hash of `ord('a') * 262 + ord('a') * 26 + ord('c')`. 

##### Knuth-Morris-Pratt:

```

```



