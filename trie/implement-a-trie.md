#### Implement a Trie

> Implement a trie with `insert`, `search`, and `startsWith` methods.
>
> **Example:**
>
> ```
> Trie trie = new Trie();
>
> trie.insert("apple");
> trie.search("apple");   // returns true
> trie.search("app");     // returns false
> trie.startsWith("app"); // returns true
> trie.insert("app");   
> trie.search("app");     // returns true
> ```

##### Map:

```py
class Trie:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.root = {}

    def insert(self, word: 'str') -> 'None':
        """
        Inserts a word into the trie.
        """
        node = self.root
        for c in word:
            if c not in node:
                node[c] = {}
            node = node[c]
        # Marks that this is a word
        node['#'] = '#'

    def search(self, word: 'str') -> 'bool':
        """
        Returns if the word is in the trie.
        """
        node = self.root
        for c in word:
            if c not in node:
                return False
            node = node[c]
        return '#' in node

    def startsWith(self, prefix: 'str') -> 'bool':
        """
        Returns if there is any word in the trie that starts with the given prefix.
        """
        node = self.root
        for c in prefix:
            if c not in node:
                return False
            node = node[c]
        return True
```



