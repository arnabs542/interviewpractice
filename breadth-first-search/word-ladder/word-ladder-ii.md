#### Word Ladder II

> Given two words \(_beginWord_ and _endWord_\), and a dictionary's word list, find all shortest transformation sequence\(s\) from _beginWord_ to _endWord_, such that:
>
> 1. Only one letter can be changed at a time
> 2. Each transformed word must exist in the word list. Note that _beginWord_ is _not_ a transformed word.
>
> **Note:**
>
> * Return an empty list if there is no such transformation sequence.
> * All words have the same length.
> * All words contain only lowercase alphabetic characters.
> * You may assume no duplicates in the word list.
> * You may assume _beginWord_ and _endWord_ are non-empty and are not the same.
>
> **Example 1:**
>
> ```
> Input:
> beginWord = "hit",
> endWord = "cog",
> wordList = ["hot","dot","dog","lot","log","cog"]
>
> Output:
> [
>   ["hit","hot","dot","dog","cog"],
>   ["hit","hot","lot","log","cog"]
> ]
> ```
>
> **Example 2:**
>
> ```
> Input:
> beginWord = "hit"
> endWord = "cog"
> wordList = ["hot","dot","dog","lot","log"]
>
> Output: 
> []
>
> Explanation: The endWord "cog" is not in wordList, therefore no possibletransformation.
> ```

##### DFS \(TLE\):

```py
def findLadders(beginWord: 'str', endWord: 'str', wordList: 'List[str]') -> 'List[List[str]]':
        
    wordset = set(wordList)
    res = []

    def dfs(start, end, path, used):
        nonlocal res
        if path[-1] == end:
            if not res or len(res[0]) > len(path):
                res = [path[:]]
            elif len(res[0]) == len(path):
                res.append(path[:])
            return
        for k in range(len(start)):
            for c in string.ascii_lowercase:
                word = start[:k] + c + start[k+1:]
                if word in wordset and word not in used:
                    used.add(word)
                    path.append(word)
                    dfs(word, end, path, used)
                    path.pop()
                    used.discard(word)

    dfs(beginWord, endWord, [beginWord], set())
    return res
```

The brute force solution is to simply perform a DFS search with all 26 letters at each seed word. This quickly times out. 

