#### Palindrome Partitioning II

> Given a string _s_, partition _s_ such that every substring of the partition is a palindrome.
>
> Return the minimum cuts needed for a palindrome partitioning of _s_.
>
> **Example:**
>
> ```
> Input: "aab"
>
> Output: 1
>
> Explanation: The palindrome partitioning ["aa","b"] could be produced using 1 cut.
> ```

DP Solution \(TLE\):

```py
def minCut(s):

    is_palindrom = [1] + [0] * len(s)
    elems = [0] + [len(s)] * len(s)
    is_palindrom[0] = 1

    min_split = len(s)

    for i in range(len(s)):
        for j in range(i, -1, -1):       
            if s[j:i+1] == s[j:i+1][::-1]:                
                elems[i+1] = min(elems[i+1], 1 + elems[j])

    return elems[-1] - 1
```



