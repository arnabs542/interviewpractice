#### Distinct Subsequences II

> Given a string `S`, count the number of distinct, non-empty subsequences of `S` .
>
> Since the result may be large, **return the answer modulo **`10^9 + 7`.
>
> **Example 1:**
>
> ```
> Input: "abc"
> Output: 7
> Explanation: The 7 distinct subsequences are "a", "b", "c", "ab", "ac", "bc", and "abc".
> ```
>
> **Example 2:**
>
> ```
> Input: "aba"
> Output: 6
>
> Explanation: The 6 distinct subsequences are "a", "b", "ab", "ba", "aa" and "aba".
> ```
>
> **Example 3:**
>
> ```
> Input: "aaa"
> Output: 3
>
> Explanation: The 3 distinct subsequences are "a", "aa" and "aaa".
> ```

##### Brute Force:

```py
def distinctSubseqII(S):

    s = set()

    def helper(start, S, cur):
        if start >= len(S):
            if len(cur) > 0:
                s.add(cur)
            return
        # include current letter
        helper(start + 1, S, cur + S[start])
        # don't include current letter
        helper(start + 1, S, cur)

    helper(0, S, "")
    return len(s)
```

The brute force solution simply generates all possible subsequences, adding them to a set, and then seeing how many elements are in the set. This method requires $$\small \mathcal O(2^{n})$$ time and times out very quickly.

##### Dynamic Programming:

```py
def distinctSubseqII(S):

    endswith = [0] * 26        
    for char in S:
        endswith[ord(char) - ord('a')] = sum(endswith) + 1

    return sum(endswith) % (10**9 + 7)
```

The idea behind this solution is a bit unintuitive. 

