#### Smallest Subsequence of Distinct Characters

> Return the lexicographically smallest subsequence of text that contains all the distinct characters of text exactly once.
>
>Example 1:
> ```
> Input: "cdadabcc"
> Output: "adbc"
>```
> Example 2:
>
>```
> Input: "abcd"
> Output: "abcd"
>```
> Example 3:
>```
> Input: "ecbacba"
> Output: "eacb"
>```
> Example 4:
>```
> Input: "leetcode"
> Output: "letcod"
> ```
>
> Note:
> * 1 <= text.length <= 1000
> * text consists of lowercase English letters.

##### Solution

**Intuition:** Since we are looking for a subsequence, that means the relative order between the letters must be preserved. Consider the example `s = 'ecbacaba'`. The answer is `eacb`, because the last (and first) appearance of `e` occurs before we see any `a` characters. 

This is the restraint we are dealing with: we can try to put letters in their alphabetical order as long as they appear after any preceding letters. From the input string, we are trying to greedily build a monotonically increasing result string. If the input character is smaller than the back of the result string, we remove larger characters from the back **provided** there are more occurrences of that character in the input string.

We use a combination of a dictionary and a stack to help us build our result:

```py
def smallestSubsequence(text: str) -> str:
    count = collections.Counter(text)
    stack, used = [], collections.defaultdict(int)

    for char in text:
        count[char] -= 1
        used[char] += 1
        if used[char] > 1:
            continue
        while stack and stack[-1] > char and count[stack[-1]] > 0:
            used[stack[-1]] = 0
            stack.pop()

        stack.append(char)

    return "".join(stack)
```

The running time of this algorithm is bounded iterating through the string, giving us a time bound of $\small \mathcal O(n)$.