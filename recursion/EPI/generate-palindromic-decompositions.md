#### Generate Palindromic Decompositions

> A string is said to be palindromic if it reads the same backwards and forwards. A decomposition of a string is a set of strings whose concatenation is the string. For example `"611116"` is a palindromic, and `"611", "11", "6"` is one decomposition for it. 
>
> Compute all palindromic decompositions of a given string. For example, if the string is `"0204451881"`, then the decomposition `"020"`, `"44"`, `"5"`, `"1881"` is palindromic, as is `"020"`, `"44"`, `"5"`, `"1"`, `"88"`, `"1"`. However, `"02044"`, `"5"`, `"1881"` is not. 

##### Solution

The brute force solution is to iterate through the string, and whenever we find a palindromic prefix, cut it off and recurse on the rest of the string:

```py
def palindrome_decompositions(s):
    if not s:
        return [[]]
    
    res = []
    for i in range(1, len(s) + 1):
        if s[:i] == s[:i][::-1]:
            for x in palindrome_decompositions(s[i:]):
                res.append([s[:i]] + x)
    return res
```

The book solution is actually quite similar, except it passes the prefix down:

```py
def palindrome_decompositions(s):

    def directed_decomp(offset, partial_partition):
        print(partial_partition)
        if offset == len(s):
            res.append(list(partial_partition))
            return
        
        for i in range(offset + 1, len(s) + 1):
            prefix = s[offset:i]
            if prefix == prefix[::-1]:
                directed_decomp(i, partial_partition + [prefix])

    res = []
    directed_decomp(0, [])
    return res
```

The running time of this solution is $\small \mathcal O(n*2^{n})$. There are $\small \mathcal O(2^{n})$ possible decompositions for the string, and for each decomposition, it takes $\small \mathcal O(n)$ time to verify whether or not it's a palindrome. 