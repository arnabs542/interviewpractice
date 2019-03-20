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

A string of $$\small n$$ characters gives us $$\small n-1 = \mathcal O(n)$$ positions to make our cut, resulting in a runtime of $$\small \mathcal O(2^{n})$$. This does not factor in the time taken to splice and rebuild the substring. Obviously, this quickly times out for large inputs.

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



