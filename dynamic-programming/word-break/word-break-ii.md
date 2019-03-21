#### Word Break II

> Given a **non-empty** string _s_ and a dictionary _wordDict_ containing a list of **non-empty** words, add spaces in _s_ to construct a sentence where each word is a valid dictionary word. Return all such possible sentences.
>
> **Note:**
>
> * The same word in the dictionary may be reused multiple times in the segmentation.
> * You may assume the dictionary does not contain duplicate words.
>
> **Example 1:**
>
> ```
> Input:
>
> s = "catsanddog"
> wordDict = ["cat", "cats", "and", "sand", "dog"]
>
> Output:
>
> [
>   "cats and dog",
>   "cat sand dog"
> ]
> ```
>
> **Example 2:**
>
> ```
> Input:
>
> s = "pineapplepenapple"
> wordDict = ["apple", "pen", "applepen", "pine", "pineapple"]
>
> Output:
>
> [
>   "pine apple pen apple",
>   "pineapple pen apple",
>   "pine applepen apple"
> ]
>
> Note that you are allowed to reuse a dictionary word.
> ```
>
> **Example 3:**
>
> ```
> Input:
>
> s = "catsandog"
> wordDict = ["cats", "dog", "sand", "and", "cat"]
>
> Output:
>
> []
> ```

##### Dynamic Programming:

```py
def reconstruct(dp: "Dict[List]", idx: "int", s:"str", decomp: "List[str]", decomps: "List[str]") -> "None":
    if idx < 0:
        decomps.append(" ".join(decomp[::-1]))
        return
    for length in dp[idx]:
        decomp.append(s[idx-length+1: idx+1])
        self.reconstruct(dp, idx-length, s, decomp, decomps)
        decomp.pop()

def wordBreak(s: str, wordDict: "List[str]") -> "List[str]":
    dp = collections.defaultdict(list)
    wordDict = set(wordDict)

    for i in range(len(s)):
        if s[:i+1] in wordDict:
            dp[i].append(i+1)
        for j in range(i):
            if j in dp and s[j+1:i+1] in wordDict:
                dp[i].append(i - j)

    decomps = []
    if len(s) - 1 in dp:
        self.reconstruct(dp, len(s) - 1, s, [], decomps)
    return decomps
```

This solution is a slightly modified of the **BedBathandBeyond.com** problem. There are two differences:

1. When we find a prefix that in its entirety is a word, we don't continue.
2. Instead of a 1D list storing the length of a single word, we use a `Dict[List]` to store the lengths of all words that end at the current index.

We don't continue when we find a prefix that is itself a word because that prefix could also be decomposed. For example, in the second example, the prefix `pineapple` is a word, but it can also be broken into `"pine apple"`.



