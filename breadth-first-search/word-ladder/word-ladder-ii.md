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
>   ["hit","hot","lot","log","cog"]
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
> Explanation: The endWord "cog" is not in wordList, therefore no possibletransformation.
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

The brute force solution is to simply perform a DFS search with all 26 letters at each seed word. I think running time is $$\small \mathcal O(n*l*26)$$, where $$\small n,l$$ are the sizes of the arrays and words. The depth of the DFS search is bounded by $$\small n$$, since we won't search more than all the words in the array. For each word, we loop through and replace one letter at a time with all 26 possible letters in the alphabet.

##### Single-sided BFS:

```py
def findLadders(beginWord, endWord, wordList):

    wordList = set(wordList)
    res = []
    chains = {}
    chains[beginWord] = [[beginWord]]

    while chains:
        next_chains = collections.defaultdict(list)
        for w in chains:
            if w == endWord:
                res = chains[endWord]
                return res
            else:                
                for i in range(len(w)):
                    for c in string.ascii_lowercase:
                        new_word = w[:i] + c + w[i+1:]
                        if new_word in wordList:
                            next_chains[new_word] += [j + [new_word] for j in chains[w]]

        wordList -= set(next_chains.keys())
        chains = next_chains

    return []
```

The main idea of the above algorithm is to keep track of the chain that led to each word as we transform them. For example, suppose the input is:

```
beginWord = "hit",
endWord = "cog",
wordList = ["hot","dot","dog","lot","log","cog"]
```

Our chain will transform as such:

```
{'hot': [['hit', 'hot']]})
{'dot': [['hit', 'hot', 'dot']], 'lot': [['hit', 'hot', 'lot']]})
{'dog': [['hit', 'hot', 'dot', 'dog']], 'log': [['hit', 'hot', 'lot', 'log']]})
{'cog': [['hit', 'hot', 'dot', 'dog', 'cog'], ['hit', 'hot', 'lot', 'log', 'cog']]})
```

##### Double-sided BFS:

```py
class Solution:
    def __adjacentWords(self, word, wordSet):
        for i in range(len(word)):
            p1, p2 = word[:i], word[i + 1:]
            for c in string.ascii_lowercase:
                w = p1 + c + p2
                if w in wordSet:
                    yield w

    def __generatePath(self, beginWord, endWord, parents):
        result = [[endWord]]
        while result[0][0] != beginWord:
            result = [[p] + r for r in result for p in parents[r[0]]]
        return result

    def findLadders(self, beginWord, endWord, wordList):
        """
        :type beginWord: str
        :type endWord: str
        :type wordList: List[str]
        :rtype: List[List[str]]
        """
        wordSet = set(wordList)
        if endWord not in wordSet:
            return []

        front, back = {beginWord}, {endWord}
        parents = collections.defaultdict(list)

        while True:
            layer = front
            if len(front) > len(back):
                layer = back
            if not layer:
                break
            wordSet -= layer

            nextLayer = set()
            for w in layer:
                for a in self.__adjacentWords(w, wordSet):
                    nextLayer.add(a)
                    if layer == front:
                        parents[a].append(w)
                    else:
                        parents[w].append(a)
            if layer == front:
                front = nextLayer
            else:
                back = nextLayer
            if front & back:
                return self.__generatePath(beginWord, endWord, parents)

        return []
```

The main idea of the above algorithm is still very similar to the single-sided BFS; we use a `defaultdict(list)` to keep track of all the parents of each word. The main difference is that instead of keeping track of the whole transformation chains, we simply keep track of the immediate preceding words.

We work from both the starting and ending work, switching to whichever currently has fewer elements. For example, suppose the input is:

```
beginWord = "hit",
endWord = "cog",
wordList = ["hot","dot","dog","lot","log","cog"]
```

The elements `front`, `back`, and `parents` will evolve as follows:

```
{'hit'} {'cog'} {}
{'hot'} {'cog'} {'hot': ['hit']}
{'lot', 'dot'} {'cog'} {'hot': ['hit'], 'dot': ['hot'], 'lot': ['hot']}
{'lot', 'dot'} {'dog', 'log'} {'hot': ['hit'], 'dot': ['hot'], 'lot': ['hot'], 'cog': ['dog', 'log']}
```

When there is an overlap between `front` and `back`, then we know we've found the shortest transformation\(s\) and can rebuild the path\(s\). To do so, we begin with the end word, then for each parent it has, we create a new list with that parent in front. We recursively do this until we reach the starting word.

