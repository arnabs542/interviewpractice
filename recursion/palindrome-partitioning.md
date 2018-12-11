#### Palindrome Partitioning

> Given a string _s_, partition _s_ such that every substring of the partition is a palindrome.
>
> Return all possible palindrome partitioning of _s_.
>
> **Example:**
>
> ```
> Input: "aab"
>
> Output:
> [
>   ["aa","b"],
>   ["a","a","b"]
> ]
> ```

##### Backtracking:

```py
def partition(s):

    res = []

    def helper(start, s, cur, res):
        if start == len(s):
            res.append(cur[:])
            return
        for i in range(start+1, len(s)+1):
            if s[start:i] == s[start:i][::-1]:
                cur.append(s[start:i])
                helper(i, s, cur, res)
                cur.pop()

    helper(0, s, [], res)
    return res
```

This is just a standard backtracking/dfs approach to solving the problem. Iterate through the array until we find a palindrome, then break that off and try to split the rest of the string into palindromic substrings as well. 

