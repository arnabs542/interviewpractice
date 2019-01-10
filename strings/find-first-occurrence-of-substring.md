#### Find First Occurrence of Substring

> Given a string and a pattern, find the starting indices of all occurrences of the pattern in the string. For example, given the string "abracadabra" and the pattern "abr", you should return `[0, 7]`.

The problem prompt from above is from a DCP question asked by Microsoft. I will solve the simpler version of finding the first occurrence of the pattern as a way of introducing the Rabin-Karp and Knuth-Morris-Pratt algorithms commonly used for string matching.

##### Brute Force:

```py
def findPassern(s, p):
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

The simple way of solving the problem would be compare every substring with a length of the pattern against the pattern itself. If it matches, we're done. Runtime is bounded by $$\small \mathcal O(s*p)$$, where $$\small s,p$$ represent the lengths of the string and the pattern.

##### Rabin-Karp:

```

```

Knuth-Morris-Pratt:

```

```



