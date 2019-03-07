#### Longest Palindromic Subsequence

> Given a string s, find the longest palindromic subsequence's length in s. You may assume that the maximum length of s is 1000.
>
> **Example 1:**  
>  Input:
>
> ```
> "bbbab"
> ```
>
> Output:
>
> ```
> 4
> ```
>
> One possible longest palindromic subsequence is "bbbb".
>
> **Example 2:**  
>  Input:
>
> ```
> "cbbd"
> ```
>
> Output:
>
> ```
> 2
> ```
>
> One possible longest palindromic subsequence is "bb".

##### Recursive Solution \(TLE\):

```py
def longestPalindromeSubseq(s: str) -> int:

    def helper(i, j):
        if i > j:
            return 0
        elif i == j:
            return 1        
        if s[i] == s[j]:
            return 2 + helper(i+1, j-1)
        else:
            return max(helper(i+1, j), helper(i, j-1))

    return helper(0, len(s) - 1)
```

If we are given a string of length 0, the longest palindromic subsequence is 0. If we are given a string of length 1, the longest palindromic subsequence is 1. These are our two base cases.

If the first and last characters of the string are equal, then we can add 2 to the longest palindromic subsequence found in `s[1:-1]`. Otherwise, we have two options:

1. Find the longest palindromic subsequence in `s[:-1]`
2. Find the longest palindromic subsequence in `s[1:]`

This is where the subproblem structure comes in:

```
          L(0, 5)    
         /      \     
    L(1,5)      L(0,4)    
   /     \      /     \    
L(2,5)  L(1,4) L(1,4) L(0,3)
```

This however times out because we keep doing the same work over and over again \(runtime is $$\small \mathcal O(2^{n})$$\). To improve our time complexity, we use remember the result for `s[i:j]`.

##### Dynamic Programming \(Top-down\):

```py
def longestPalindromeSubseq(s: str) -> int:
    memo = {}

    def helper(i, j):
        if (i, j) in memo:
            return memo[(i,j)]
        elif i > j:
            return 0
        elif i == j:
            return 1

        if s[i] == s[j]:
            res = 2 + helper(i+1, j-1)
            memo[(i,j)] = res
        else:
            res = max(helper(i+1, j), helper(i, j-1), helper(i+1, j-1))
            memo[(i,j)] = res
        return res

    return helper(0, len(s) - 1)
```

The overall runtime for this solution is $$\small \mathcal O(n^{2})$$, where $$\small n$$ is the length of the string. This is because we calculate the longest palindromic subsequence in each substring once before storing the result, and there are a total of $$\small n^{2}$$ substrings.

