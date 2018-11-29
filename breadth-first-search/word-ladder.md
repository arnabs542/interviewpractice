#### Word Ladder

> Given two words \(_beginWord_ and _endWord_\), and a dictionary's word list, find the length of shortest transformation sequence from _beginWord_ to _endWord_, such that:
>
> 1. Only one letter can be changed at a time.
> 2. Each transformed word must exist in the word list. Note that _beginWord_ is _not_ a transformed word.
>
> **Note:**
>
> * Return 0 if there is no such transformation sequence.
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
> Output: 5
>
> Explanation:
>  As one shortest transformation is "hit" -> "hot" -> "dot" -> "dog" -> "cog",return its length 5.
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
>
> Output: 0
>
> Explanation: The endWord "cog" is not in wordList, therefore no possible transformation.
> ```

##### Unoptimized BFS:

```py
def ladderLength(beginWord, endWord, wordList):

    wordList = set(wordList)

    if endWord not in wordList:
        return 0

    bfs = collections.deque([beginWord])
    seen = set([beginWord])

    count = 1

    def one_apart(cur, w):
        diff = 0
        for i in range(len(cur)):
            if cur[i] != w[i]:
                diff += 1
                if diff > 1:
                    return False
        return True

    while bfs:
        n = len(bfs)
        for i in range(n):
            cur = bfs.popleft()
            for w in wordList:
                if one_apart(cur, w) and w not in seen:
                    if w == endWord:
                        return count + 1
                    seen.add(w)
                    bfs.append(w)
        count += 1

    return 0
```

The idea behind the algorithm is to get all of the possible transformations after a certain amount of steps, and then see when we reach the final word. For example, given the input:

```
beginWord = "hit",
endWord = "cog",
wordList = ["hot","dot","dog","lot","log","cog"]
```

After 1 move, we could have the words \["hot"\]. After 2 moves, we can have the words \["lot", "dot"\]. The problem with solution is that the method for determining a valid transformation takes too long - each word requires iterating through the entire array to determine next words. 

