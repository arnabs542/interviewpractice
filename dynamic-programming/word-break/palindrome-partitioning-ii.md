#### Palindrome Partitioning II

> Given a string _s_, partition _s_ such that every substring of the partition is a palindrome.
>
> Return the minimum cuts needed for a palindrome partitioning of _s_.
>
> **Example:**
>
> ```
> Input: "aab"
> Output: 1
> Explanation: The palindrome partitioning ["aa","b"] could be produced using 1 cut.
> ```

##### Dynamic Programming:

```py
def minCut(s: str) -> int:
    if not s or s == s[::-1]:
        return 0

    cuts = [i+1 for i in range(len(s))]
    
    for i in range(len(s)):
        if s[:i+1] == s[:i+1][::-1]:
            cuts[i] = 0
            continue
        for j in range(i):
            if s[j+1:i+1] == s[j+1:i+1][::-1]:
                cuts[i] = min(cuts[j] + 1, cuts[i])
    
    return cuts[-1]
```

This problem is extremely similar to Word Break. As we iterate through the string, we check if it is a palindrome. If not, we try to find the longest palindrome that ends at the current index; the string requires 1 additional cut in addition to the amount of cuts its prefix has.

Running time is $\small \mathcal O(n^{3})$, thanks to the way we check if a string is a palindrome. We can reduce this to $\small \mathcal O(n^{2})$ by changing the way we check if a string is a palindrome:

```py
def minCut(s: str) -> int:
    if not s or s == s[::-1]:
        return 0

    cuts = [i for i in range(-1, len(s))]

    for i in range(len(s)):

        r1 = r2 = 0 # r1 checks for odd palindromes, r2 checks for even palindromes

        while i-r1 >= 0 and i+r1 < len(s) and s[i-r1] == s[i+r1]:
            cuts[i+r1+1] = min(cuts[i+r1+1], 1 + cuts[i-r1])
            r1 += 1

        while i-r2 >= 0 and i+r2+1 < len(s) and s[i-r2] == s[i+r2+1]:
            cuts[i+r2+2] = min(cuts[i+r2+2], 1+cuts[i-r2])
            r2 += 1

    return cuts[-1]
```

We need an extra `[-1]` at the beginning of the `cuts` array to account for the situation when the entire \(sub\)string is a prefix. This expansion method of palindrome checking is in general much faster than using splices. 

