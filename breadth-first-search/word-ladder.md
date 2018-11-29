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

##### Optimized BFS:

```py
def ladderLength(beginWord, endWord, wordList):

    wordList = set(wordList)

    if endWord not in wordList:
        return 0

    bfs = collections.deque([beginWord])
    wordList.discard(beginWord)

    dist = 1

    while bfs:
        n = len(bfs)
        for i in range(n):
            cur_word = bfs.popleft()
            for i in range(len(cur_word)):
                for c in string.ascii_lowercase:
                    transform = cur_word[:i] + c + cur_word[i+1:]
                    print(transform)
                    if transform == endWord:
                        return dist + 1
                    elif transform in wordList:
                        wordList.discard(transform)
                        bfs.append(transform)
        dist += 1

    return 0
```

The first optimization we can make is change how we search for next possible transforms. Since we are only allowed to change one letter at a time, then we might as well try changing each letter of the current word to all 26 letters and see if that word is in the dictionary. If it is, we can then add it to our queue and remove it from the dictionary. The running time is bounded by $$\small \mathcal O(n * l * 26)$$. Since at most we will need to consider every single word, the while loop will take $$\small \mathcal O(n)$$ time, where $$\small n$$ is the number of words in the dictionary. We loop through every letter of each word, which takes $$\small \mathcal O(l)$$ time, where $$\small l$$ is the length of each word. The 26 is just more specific, since there are 26 letters.

##### Bidirectional BFS:

```py
def ladderLength(beginWord, endWord, wordList):

    wordList = set(wordList)

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

A stronger optimization is to perform BFS from both direction - we simultaneously change the starting and ending word and attempt to meet in the middle. At the end of each turn, we swap front and back if needed to ensure that front is always the shorter of the two queues. This means we can iterate through less elements the next turn. Running time doesn't change. 

