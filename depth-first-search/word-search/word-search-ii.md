#### Word Search II

> Given a 2D board and a list of words from the dictionary, find all words in the board.
>
> Each word must be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.

**Example:**

```
Input:
words = ["oath","pea","eat","rain"] and board =
[
  ['o','a','a','n'],
  ['e','t','a','e'],
  ['i','h','k','r'],
  ['i','f','l','v']
]

Output: ["eat","oath"]
```

Essentially the same problem as the above one, except now we need to find multiple word. If we simply apply the above solution to this one by looping through all the words, our time complexity is $$\small \mathcal O(m*n*s*4^{l})$$, where $$\small m, n$$ are the dimensions of the board, $$\small s$$ is the size of the words array, and $$\small l$$ is the length of the longest word. This quickly times out, which suggests we need to figure out how to prune our backtracking earlier.

##### Trie + DFS:

```py
def find(board, i, j, trie, cur_word, found):
    if '#' in trie:
        found.add("".join(cur_word))
    if not (0 <= i < len(board) and 0 <= j < len(board[0])) or board[i][j] not in trie:
        return
    cur_word.append(board[i][j])
    c, board[i][j] = board[i][j], '-'
    find(board, i+1, j, trie[c], cur_word, found)
    find(board, i, j+1, trie[c], cur_word, found)
    find(board, i-1, j, trie[c], cur_word, found)
    find(board, i, j-1, trie[c], cur_word, found)
    cur_word.pop()
    board[i][j] = c

def findWords(board, words):
    """
    :type board: List[List[str]]
    :type words: List[str]
    :rtype: List[str]
    """
    # Build a trie to contain all words in the given list
    trie = {}
    for w in words:
        t = trie
        for i in range(len(w)): # Put each word in the trie
            if w[i] not in t:
                t[w[i]] = {}
            t = t[w[i]]
        t['#'] = '#'    # Marks the end of a word

    found, cur_word = set(), []
    for i in range(len(board)):
        for j in range(len(board[0])):
            if board[i][j] in trie:
                find(board, i, j, trie, [], found)

    return list(found)
```

Compared to the previous problem, the naive solution introduces an extra factor of $$\small s$$ into the runtime. By first using a tree to store every single word, we don't need to keep looping through the word list, looking for a specific word each time. Instead, we simply perform a dfs search on every cell, and if we happen to build a word that is in the given list, then we add that to the result.

This allows us to bring the runtime back down to $$\small \mathcal O(m*n*4^{l})$$, same as the previous problem.

##### Edge Cases/Traps:

The order of the checks in `find` is actually very important - it's vital we check for `#` before checking for out of bounds/extra character, because we don't check if the last character we added actually ended a word. Thus, we could be out of bounds right now, but we could still have a valid word, thanks to the square that led us out of bounds. 

