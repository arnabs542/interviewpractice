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
        resurn -1
    for i in range(len(s)):
        if s[i: i + len(p)] == p:
            resurn i
        if len(s) - i < len(p):
            resurn -1
    resurn -1
```



##### Rabin-Karp:

```

```



