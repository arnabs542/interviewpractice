#### Can Make Palindrome from Substring

> Given a string s, we make queries on substrings of s.
>
> For each query `queries[i] = [left, right, k]`, we may rearrange the substring `s[left], ..., s[right]`, and then choose up to `k` of them to replace with any lowercase English letter. 
>
> If the substring is possible to be a palindrome string after the operations above, the result of the query is `True`. Otherwise, the result is `False`.
>
> Return an array `answer[]`, where `answer[i]` is the result of the i-th query `queries[i]`.
>
> Note that: Each letter is counted individually for replacement so if for example `s[left..right] = "aaa"`, and `k = 2`, we can only replace two of the letters.  (Also, note that the initial string s is never modified by any query.)
>
> Example :
>```
> Input: s = "abcda", queries = [[3,3,0],[1,2,0],[0,3,1],[0,3,2],[0,4,1]]
> Output: [true,false,false,true,true]
> Explanation:
> queries[0] : substring = "d", is palidrome.
> queries[1] : substring = "bc", is not palidrome.
> queries[2] : substring = "abcd", is not palidrome after replacing only 1 character.
> queries[3] : substring = "abcd", could be changed to "abba" which is palidrome. Also this can be changed to "baab" first rearrange it "bacd" then replace "cd" with "ab".
> queries[4] : substring = "abcda", could be changed to "abcba" which is palidrome.
>```

##### Solution

We begin the problem by asking how we might decide if a substring can be transformed into a palindrome. Since we're allowed to rearrange the substring, then the original substring doesn't matter, only its characters. More specifically, a substring can be rearranged to form a plalindrome if and only if there is at most one character with an odd number of appearances. 

Since we are allowed `k` replacements, we only need to repalce at most $\left \lfloor{x/2}\right \rfloor$ characters. With these two relationships established, we can first write the helper function:

```py
# Check to see if we can transform a string with r replacements        
def canChange(string, replacements):
    char_count = collections.Counter(string)
    odd_count = 0
    for c in char_count:
        if char_count[c] % 2:
            odd_count += 1
    return odd_count // 2 <= replacements
```

The naive way to check for every query is to build the substring each time we come across a new query. OBviously, this will result in us doing a lot repeated work, and is also an expensive operation time wise. We need to somehow get the character count for an arbitrarily defined substring. 

This is where the idea of prefix sum comes in, Suppse we know the character count of `s[:i]`. If we know the character count for `s[:j]`, then the character count for `s[i:j]` is simply `char_count(s[:j]) - s[:i]`. While this may seem like a difficult problem by itself at first, we can use the condition that are there are only 26 letters in the English alphabet, so we can store the character count for a string in a length 26 array. This leads us to our first accepted implementation:


```py
def canMakePaliQueries(self, s: str, queries: List[List[int]]) -> List[bool]:
    
    # Build dictionary to keep track number of odd-frequency letters between s[i:j+1]
    char_count = [0] * 26
    idx_count = collections.defaultdict(tuple)
    idx_count[0] = tuple(char_count)
    for i, c in enumerate(s):
        char_count[ord(c) - ord('a')] += 1
        idx_count[i+1] = tuple(char_count)
        
    res = []
    for start, end, replacements in queries: 
        if start == end:
            res.append(True)
        else:
            odd_count = 0
            # Find number of odd-frequency letters 
            count_end = idx_count[end+1]
            count_start = idx_count[start]
            for i in range(26):
                if (count_end[i] - count_start[i]) % 2:
                    odd_count += 1        
            res.append(odd_count // 2 <= replacements)

    return res
```

We traverse through the string once, each time caching a length 26 tuple. This takes $\small \mathcal O(26*n) = O(n)$ time. We then go through each query, get the character count of the substring, and then count the number of odd characters. This takes $\small \mathcal O(26*m) = \mathcal O(m)$ time. Overall time take is $\small \mathcal O(m*n)$ time. 

While we reached the optimal upper bound for the time complexity, we can still improve on our previous solution by using bit operation. This is because we don't actually need to store the count of each character - we only need to store 0 or 1 depending on whether it appears an odd or even number of times. 

```py
def canMakePaliQueries(self, s: str, queries: List[List[int]]) -> List[bool]:
    s_len = len(s)
    char_count = [0] * (s_len + 1)

    for i,c in enumerate(s):
        char_count[i+1] = char_count[i] ^ (1 << (ord(c) - ord('a')))

    res = []
    for left, right, replacements in queries:
        substring_char_count = char_count[right+1] ^ char_count[left]
        res.append(True if (bin(substring_char_count).count('1') // 2) <= replacements else False)

    return res
```

Instead of using an array to map our character count, we simply use an integer, which has 32 bits, more than the 26 bits needed to represent every character in the English alaphabet. First we iterate through the string, and we `xor` each character with the previous substring. This way, the count at that character index will be a `0` if it has appeared an even number of times, and `1` otherwise. 

Next to find the character count between two indicies, we simply do `char_count[i] ^ char_count[j+1]`, because the common substring `s[:i]` cancel itself out in the `xor` operation. We then convert the number to its binary form and count the number of odd characters left. 