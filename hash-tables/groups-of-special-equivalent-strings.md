#### Groups of Special-Equivalent Strings

> You are given an array `A` of strings.
>
> Two strings `S` and `T` are _special-equivalent_ if after any number of _moves_, S == T.
>
> A _move_ consists of choosing two indices `i` and `j` with `i % 2 == j % 2`, and swapping `S[i]` with `S[j]`.
>
> Now, a _group of special-equivalent strings from _`A` is a non-empty subset S of `A` such that any string not in S is not special-equivalent with any string in S.
>
> Return the number of groups of special-equivalent strings from `A`.
>
> **Example 1:**
>
> ```
> Input: ["a","b","c","a","c","c"]
> Output: 3
> Explanation: 3 groups ["a","a"], ["b"], ["c","c","c"]
> ```
>
> **Example 2:**
>
> ```
> Input: ["aa","bb","ab","ba"]
> Output: 4
> Explanation: 4 groups ["aa"], ["bb"], ["ab"], ["ba"]
> ```
>
> **Example 3:**
>
> ```
> Input: ["abc","acb","bac","bca","cab","cba"]
> Output: 3
> Explanation: 3 groups ["abc","cba"], ["acb","bca"], ["bac","cab"]
> ```
>
> **Example 4:**
>
> ```
> Input: ["abcd","cdab","adcb","cbad"]
> Output: 1
> Explanation: 1 group ["abcd","cdab","adcb","cbad"]
> ```

##### Code \(TLE\):

```py
def numSpecialEquivGroups(A):

    def is_eq(s1, s2):
        if len(s1) != len(s2):
            return False
        chars = [collections.defaultdict(int), collections.defaultdict(int)]
        for i in range(len(s1)):
            chars[i % 2][s1[i]] += 1
            chars[i % 2][s2[i]] += 1
        for k,v in chars[0].items():
            if v % 2:
                return False
        for k,v in chars[1].items():
            if v % 2:
                return False
        return True

    groups = collections.defaultdict(list)
    for s in A:
        for k in groups:
            if is_eq(k, s):
                groups[k].append(s)
                break
        else:
            groups[s].append(s)
    return len(groups)
```

##### Explanation:

My first approach time out. The idea was to process each string by running it through all other groups and see if it fit into any already processed groups. If not, then it would become its one group. The running time of the above approach is bounded by $$\small \mathcal O(N^{2}m)$$, where $$\small N, m$$ represent the number of words and the word length respectively.

The idea here is to use hashing, but in a more efficient way. The extra layer of complexity of this problem is due to the restriction in terms of which characters are swappable  - without it, simply compute how many of each letters each word has, and if two words have the same set and frequency of letters, then they are equivalent.

Since we are only allowed to swap odd indices with odd indices and even with even, we just need to change our hashing function a bit.

##### Code:

```py
def numSpecialEquivGroups(self, A):

    def encoding(string):
        hashEven = [0] * 26
        hashOdd = [0] * 26
        for i,c in enumerate(string):
            if i % 2:
                hashOdd[ord(c) - ord('a')] += 1
            else:
                hashEven[ord(c) - ord('a')] += 1

        encoding = ""
        for i in range(26):
            encoding += str(hashEven[i])
            encoding += "-"
            encoding += str(hashOdd[i])
            encoding += "-"

        return encoding 

    groups = set()
    res = 0
    for s in A:
        pattern = encoding(s)
        if pattern not in groups:
            res += 1
        groups.add(pattern)
    return res
```

##### Explanation:

This idea is the same, simply record how how many characters each word has, and their positions. If two words have the same characters and the same amount of characters in both odd and even indices, then the two words are equivalent. This algorithm takes $$\small \mathcal O(Nm)$$ time, and $$\small O(N)$$ space at worst.

