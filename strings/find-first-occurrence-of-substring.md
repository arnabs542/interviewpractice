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
def rabin_karp(s, p):
    if len(p) > len(s):
        return -1

    BASE = 26
    s_hash = functools.reduce(lambda h, c: h * BASE + ord(c), s[:len(p)], 0)
    p_hash = functools.reduce(lambda h, c: h * BASE + ord(c), p, 0)
    power_p = BASE**max(len(p) - 1, 0)

    for i in range(len(p), len(s)):
        if s_hash == p_hash and s[i-len(p):i] == p:
            return i - len(p)

        # Use rolling hash to update hash code
        s_hash -= ord(s[i - len(p)]) * power_p
        s_hash = s_hash * BASE + ord(s[i])

    # Tries to match p and s[-len(p):]
    if s_hash == p_hash and s[-len(p):] == p:
        return len(s) - len(p)

    return -1
```

The Rabin-Karp algorithm relies on hashing to reduce the amount of checks. If the current substring isn't hash-equivalent to the pattern, then we automatically move on without checking.

The hashing provided above is simply  a base change. We take the ASCII value of the character at each position, and multiply it by 26 to the power for the position. For example, a string of `'abc'` would result in a hash of `ord('a') * 262 + ord('a') * 26 + ord('c')`.

While theoretically the runtime of Rabin-Karp is still bounded $$\small \mathcal O(s*p)$$, in reality the runtime is much closer to $$\small \mathcal O(s+p)$$, unless the hashing function is particularly bad and causes a lot of false positives.

##### Knuth-Morris-Pratt:

```

```

The central idea of the KMP algorithm is the usage of an auxiliary array $$\small \pi$$ that tells us where we should begin our next search in the event of a mismatch. The value at $$\small \pi[i]$$ is the largest integer smaller than $$\small i$$ such that $$\small P_{1}...P_{\pi[i]}$$ is a suffix of $$\small P_{1}...P_{i}$$.

![](/assets/KMP_pi_array.png)

Above is an example of a given pattern and the associating array with it:

* $$\small \pi[6] = 4$$ because $$\small \text{abab}$$ is a suffix of $$\small \text{ababab}$$
* $$\small \pi[9] = 0$$ since there is no prefix of length ≤ 8 that ends with $$\small \text{c}$$

Let the string $$\small s = \text{ABC ABCDAB ABCDABCDABDE}$$, and $$\small p = \text{ABCDABD}$$. $$\small \pi = (0,0,0,0,1,2,0)$$.

We begin matching at the first position of $$\small s$$:

![](/assets/KMP_1.png)

We see there is a mismatch at the 4th letter of $$\small p$$. We've matched 3 letters so far, and $$\small \pi[3] = 0$$. 



