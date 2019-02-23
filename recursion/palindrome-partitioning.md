#### Palindrome Partitioning

> Given a string _s_, partition _s_ such that every substring of the partition is a palindrome.
>
> Return all possible palindrome partitioning of _s_.
>
> **Example:**
>
> ```
> Input: "aab"
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

Time complexity analysis is a little tricky. The recurrence can be modeled as such:

$$\small T(n) = P(1) + T(n-1) + P(2) + T(n-2) + ... + P(n-1) + T(1) + P(n) + T(0)$$

$$\small = T(n-1) + T(n-2) + ... + T(1) + T(0) + P(n) + P(n-1) + ... + P(1) + P(0)$$

$$\small T(n-1) = T(n-2) + T(n-3) + ... + T(1) + T(0) + P(n-1) + P(n-2) + ... + P(1) + P(0) $$

Hence $$\small T(n) = T(n-1) + T(n-1) + P(n) = 2*T(n-1) + P(n)$$.

This solves to $$\small O(2^{n})$$. However, the overhead needed to actually copy the substring to the final array should probably bump the bound to something like $$\small \mathcal O(n*2^{n})$$.

Another way to analyze the time complexity is as follows: a string of $$\small n$$ characters will have $$\small n-1$$ positions for partitioning. At each space, we can choose to cut or not cut, giving use a total of $$\small 2^{n-1}$$ possible partitions. Each partition requires $$\small \mathcal O(n)$$ time to verify, resulting a total time complexity of $$\small \mathcal O(n*2^{n})$$.

