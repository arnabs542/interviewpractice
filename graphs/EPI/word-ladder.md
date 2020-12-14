#### Word Ladder

> Given two words (`beginWord` and `endWord`), and a dictionary's word list, find the length of shortest transformation sequence from `beginWord` to `endWord`, such that:
>
> * Only one letter can be changed at a time.
> * Each transformed word must exist in the word list. Note that `beginWord` is not a transformed word.
>
> Note:
> * Return 0 if there is no such transformation sequence.
> * All words have the same length.
> * All words contain only lowercase alphabetic characters.
> * You may assume no duplicates in the word list.
> * You may assume `beginWord` and `endWord` are non-empty and are not the same.
>
> Example 1:
> ```
> Input:
> beginWord = "hit",
> endWord = "cog",
> wordList = ["hot","dot","dog","lot","log","cog"]
> 
> Output: 5
> ```
> Explanation: As one shortest transformation is "hit" -> "hot" -> "dot" -> "dog" -> "cog", return its length 5.
>
> Example 2:
> ```
> Input:
> beginWord = "hit"
> endWord = "cog"
> wordList = ["hot","dot","dog","lot","log"]
>
> Output: 0
> ```
> Explanation: The endWord "cog" is not in wordList, therefore no possible transformation.

#### Solution

Since we're looking for the shortest path possible, we're going to use BFS: 

```py
def ladder_length(beginWord: str, endWord: str, wordList: "List[str]") -> int:
    wordList = set(wordList)
    
    ladder = 1
    bfs = collections.deque([beginWord])
    
    while bfs:
        n = len(bfs)
        for _ in range(n):
            cur_word = bfs.popleft()
            if cur_word == endWord:
                return ladder
            for i in range(len(cur_word)):
                for c in string.ascii_lowercase:
                    next_word = cur_word[:i] + c + cur_word[i+1:]
                    if next_word in wordList:
                        wordList.discard(next_word)
                        bfs.append(cur_word[:i] + c + cur_word[i+1:])
        ladder += 1
    
    return 0
```

A BFS runs in $\small \mathcal O(|V| + |E|)$ time; let $\small d$ be the number of vertices (words). Worst case scenario there are $\small d^{2}$ number of edges, meaning our algorithm will run in $\small \mathcal O(d + d^{2}) = O(d^{2})$ time. If the string length $\small n$ is less than $\small d$ then the maximum number of edges out of a vertex is $\small \mathcal O(n)$ and implies an $\small \mathcal O(nd)$ bound.

By serach from both the start and the end, we can reduce the amount of time (not overall bound):

```py
def ladder_length(beginWord: str, endWord: str, wordList: "List[str]") -> int:

    if endWord not in wordList:
        return 0
    
    front, back = collections.deque([beginWord]), collections.deque([endWord])
    wordList.discard(beginWord)
    wordList.discard(endWord)

    dist = 1

    while front:
        dist += 1
        n = len(front)
        for i in range(n):
            cur_word = front.popleft()
            for i in range(len(cur_word)):
                for c in string.ascii_lowercase:
                    transform = cur_word[:i] + c + cur_word[i+1:]
                    if transform in back:
                        return dist
                    elif transform in wordList:
                        wordList.discard(transform)
                        front.append(transform)
                        
        if len(front) > len(back):
            front, back = back, front

    return 0
```