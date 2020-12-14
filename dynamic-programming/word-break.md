#### Word Break

> Given a **non-empty** string _s_ and a dictionary _wordDict_ containing a list of **non-empty** words, determine if _s_ can be segmented into a space-separated sequence of one or more dictionary words.
>
> **Note:**
>
> * The same word in the dictionary may be reused multiple times in the segmentation.
> * You may assume the dictionary does not contain duplicate words.
>
> **Example 1:**
>
> ```
> Input: s = "leetcode", wordDict = ["leet", "code"]
>
> Output: true
>
> Explanation: 
> Return true because "leetcode" can be segmented as "leet code".
> ```
>
> **Example 2:**
>
> ```
> Input: s = "applepenapple", wordDict = ["apple", "pen"]
>
> Output: true
>
> Explanation: 
> Return true because "applepenapple" can be segmented as "apple pen apple".
> Note that you are allowed to reuse a dictionary word.
> ```
>
> **Example 3:**
>
> ```
> Input: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
>
> Output: false
> ```

##### Recursion \(TLE\):

```py
def wordBreak(s: str, wordDict: List[str]) -> bool:

    wordDict = set(wordDict)

    def helper(s):
        if not s:
            return True
        for i in range(len(s)+1):
            if s[:i] in wordDict:
                if helper(s[i:]):
                    return True
        return False
    return helper(s)
```

The brute force solution is to advance along the string, and as we encounter a prefix that is a word in the dictionary, we break at that point, and then try to split the rest of the string.

A string of $\small n$ characters gives us $\small n-1 = \mathcal O(n)$ positions to make our cut, resulting in a runtime of $\small \mathcal O(2^{n})$. This does not factor in the time taken to splice and rebuild the substring. Obviously, this quickly times out for large inputs.

##### Dynamic Programming:

```py
def wordBreak(s: str, wordDict: List[str]) -> bool:

    wordDict = set(wordDict)
    memo = [True] + [False] * len(s)

    for i in range(1, len(memo)):
        for j in reversed(range(1, i+1)):
            if s[j-1:i] in wordDict and memo[j-1]:
                memo[i] = True
                break

    return memo[-1]
```

The idea of the solution above is that each time we advance along the array, we work backwards to see if there is a dictionary word that we can build with the last character of the current substring. If there is, and the prefix can also be split into words, then `memo[i] = True`, meaning that we can split `s[:i+1]` into dictionary words. We need to be careful of how we access indices, since the length of our dp array is different from the length of our string. 

The nested for loop means our time complexity is $\small \mathcal O(n^{2})$, however the string splicing takes additional time. 

