#### Implement PrefixMapSum

> Implement a `PrefixMapSum` class with the following methods:
>
> * `insert(key: str, value: int)`: Set a given key's value in the map. If the key already exists, overwrite the value.
> * `sum(prefix: str)`: Return the sum of all values of keys that begin with a given prefix.
>
> For example, you should be able to run the following code:
>
> ```
> mapsum.insert("columnar", 3)
> assert mapsum.sum("col") == 3
>
> mapsum.insert("column", 2)
> assert mapsum.sum("col") == 5
> ```

##### Trie + Map:

```py
class PrefixMapSum:

    def __init__(self):
        self.root = {"value": 0}
        self.words = {}

    def find(self, word):
        return self.words.get(word, 0)

    def insert(self, word, value):
        val = self.words.get(word, 0)
        self.words[word] = value
        node = self.root
        for c in word:
            node['value'] += (value - val)
            if c not in node:
                node[c] = {}
                node[c]['value'] = 0
            node = node[c]
        node['value'] += (value - val)
        node['#'] = '#'        


    def sum(self, prefix):
        node = self.root
        for c in prefix:
            if c not in node:
                return 0
            node = node[c]
        return node['value']
```

We use a Trie to quickly access the prefixes. In addition to the character maps, each trie node also has a "value" key that stores the total value of all words with a prefix up to that particular character.

We use an extra dictionary to store previously stored words so we can change values quickly as we iterate through the trie.

