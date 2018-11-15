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

The array `endswith[26]` counts how many sub sequence that ends with `i`th character in the alphabet, i.e. `endswith[0]` counts how many sub sequences end with a. Suppose we have `N = sum(endswith)` different sub sequence. If we add a new character `c` to each of them, then we have `N` different sub sequence that ends with `c`. With this idea, we loop on the whole string `S`,  
 and we update `end[c] = sum(end) + 1` for each character. We need to plus one here, because `"c"` itself is also a sub sequence.

**Example:**

Input: `"aba"`  
Current parsed: `"ab"`

`endswith[a]` : `["a"]`  
`endswith[b]` : `["ab","b"]`

`"a"` -&gt;`"aa"`  
`"ab"` -&gt;`"aba"`  
`"b"` -&gt;`"ba"`  
`""` -&gt;`"a"`

`endswith[a]`: `["aa","aba","ba","a"]`  
`endswith[b]`: `["ab","b"]`  
Total: 6

Time complexity is bounded by $$\small \mathcal O(26 * n)$$, and space bounded by $$\small \mathcal O(1)$$. We can reduce time complexity to pure $$\small \mathcal O(n)$$ by utilizing an extra variable to avoid constantly calling `sum(endswith)`.

