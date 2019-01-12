#### Palindrome Partitioning II

> Given a string _s_, partition _s_ such that every substring of the partition is a palindrome.
>
> Return the minimum cuts needed for a palindrome partitioning of _s_.
>
> **Example:**
>
> ```
> Input: "aab"
>
> Output: 1
>
> Explanation: The palindrome partitioning ["aa","b"] could be produced using 1 cut.
> ```

##### DP Solution \(TLE\):

```py
def minCut(s):

    elems = [0] + [float('inf')] * len(s)

    min_split = len(s)

    for i in range(len(s)):
        for j in range(i, -1, -1):       
            if s[j:i+1] == s[j:i+1][::-1]:                
                elems[i+1] = min(elems[i+1], 1 + elems[j])

    return elems[-1] - 1
```

The idea is similar to Word Split. We maintain an extra array that tells us how many elements are in the answer if we split the array at that point. For the example s = "aab", the initial state is below:

```
elems = [0 inf inf inf]
    s = [   a   a   b ]
```

We then start iterating `i`, while backwards iterating `j`. Since we want to minimize the number of cuts, we need to extend `j` as far back as possible such that `s[:j]` and `s[j:i]` are both palindromes.

```
            i
elems = [0 inf inf inf]
    s = [   a   a   b ] 

                i
elems = [0  0  inf inf]
    s = [   a   a   b ]

                    i
elems = [0  0   0   1]
    s = [   a   a   b ]
```

The first `a` is by itself an element, and that requires no cuts since the empty string requires no cuts. Since `aa` is also a palindrome, the first two characters require no cuts. The third character `b` requires a cut, since we can't tack it onto the first palindrome. Therefore, the number of cuts for `elems[3] = 1 + elems[2]`. The overall runtime of this solution is bounded by $$\small \mathcal O(n^{3})$$. We have a nested for loop that partitions the string, but the palindrome check takes another $$\small \mathcal O(n)$$ factor.

However, if we simply add a few checks that guard against extra long palindromic inputs, or inputs that can be partitioned with just 1 split, the code passes:

```py
def minCut(s):

    if s == s[::-1]: 
        return 0
    for i in range(1, len(s)):
        if s[:i] == s[:i][::-1] and s[i:] == s[i::][::-1]:
            return 1

    elems = [0] + [float('inf')] * len(s)

    min_split = len(s)

    for i in range(len(s)):
        for j in range(i, -1, -1):       
            if s[j:i+1] == s[j:i+1][::-1]:                
                elems[i+1] = min(elems[i+1], 1 + elems[j])

    return elems[-1] - 1
```

##### Dynamic Programming:

```py
def minCut(self, s):
    """
    :type s: str
    :rtype: int
    """
    n = len(s)
    splits = [-1] + [i for i in range(n)]

    for i in range(n):
        r1 = r2 = 0            
        # Odd palindrome
        while i-r1 >= 0 and i+r1 < n and s[i-r1] == s[i+r1]:            
            splits[i+r1+1] = min(splits[i+r1+1], 1 + splits[i-r1])
            r1 += 1

        # Even palindrome
        while i-r2 >= 0 and i+r2+1 < n and s[i-r2] == s[i+r2+1]:          
            splits[i+r2+2] = min(splits[i+r2+2], 1 + splits[i-r2])
            r2 += 1

    return splits[-1]
```



