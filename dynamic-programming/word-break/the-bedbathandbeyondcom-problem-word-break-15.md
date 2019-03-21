#### The BedBathandBeyond.com Problem \(Word Break 1.5\)

> Suppose you are designing a search engine. In addition to getting keywords from a page's content, you would like to get keywords from Uniform Resource Locators \(URLs\). For example, bedbathandbeyond.com yields the keywords "bed, bath, beyond, bat, hand": the first two coming from the decomposition "bed bath beyond" and the latter two coming from the decomposition "bed bat hand beyond".
>
> Given a dictionary, i.e. a set of strings, and a name, design an efficient algorithm that checks whether the name is the concatenation of a sequence of dictionary words. If such a concatenation exists, return it. A dictionary word may appear more than once in the sequence. For example, "a", "man", "a", "plan", "a", "canal", is a valid sequence for "amanaplanacanal".

##### Solution:

The solution is quite similar to that of Word Break, the main difference being we store an integer value representing the length of the last dictionary word in the decomposition. For example, suppose the given domain was `bedbathandbeyond.com` and the dictionary was `['bed', 'bat', 'hand', 'beyond']`. The resulting dp array would look like this:

```
  b   e  d   b   a  t  h   a   n  d   b   e   y   o   n  d
[-1, -1, 3, -1, -1, 3, 4, -1, -1, 4, -1, -1, -1, -1, -1, 6]
```

In Word Break, the entry at `dp[i]` indicated whether or not `s[:i]` could be decomposed into dictionary words \(ignoring index mismatches for now\). Here, a value not equal to -1 indicates not only can the string be split at that index, but how long the dictionary word is that ends at that index. This allows us to solve both the problem of determining whether a string can be decomposed, and rebuild a single decomposition:

```py
def decompose_into_dictionary_words(domain, dictionary):

    # When the algorithm finishes, last_length[i] != -1 indicates domain[:i +
    # 1] has a valid decomposition, and the length of the last string in the
    # decomposition is last_length[i].
    last_length = [-1] * len(domain)
    for i in range(len(domain)):
        # If domain[:i + 1] is a dictionary word, set last_length[i] to the
        # length of that word.
        if domain[:i + 1] in dictionary:
            last_length[i] = i + 1
            continue

        # If domain[:i + 1] is not a dictionary word, we look for j < i such
        # that domain[: j + 1] has a valid decomposition and domain[j + 1:i + 1]
        # is a dictionary word. If so, record the length of that word in
        # last_length[i].
        for j in range(i):
            if last_length[j] != -1 and domain[j + 1:i + 1] in dictionary:
                last_length[i] = i - j
                break

    decompositions = []
    if last_length[-1] != -1:
        # domain can be assembled by dictionary words.
        idx = len(domain) - 1
        while idx >= 0:
            decompositions.append(domain[idx + 1 - last_length[idx]:idx + 1])
            idx -= last_length[idx]
        decompositions = decompositions[::-1]
    return decompositions
```

The derivations for time complexity is as follows: let $$\small n$$ be the length of the input string $$\small s$$. For each $$\small k < n$$ we check for each $$\small j < k$$ whether the substring $$\small s[j+1,k]$$ is a dictionary word, and each such check requires $$\small \mathcal O(k-j)$$ time. This implies the time complexity is $$\small \mathcal O(n^{3})$$. If we restrict $$\small j$$ to range from $$\small k-W$$ to $$\small k-1$$, where $$\small W$$ is length of the longest word, we can reduce the time complexity to $$\small \mathcal O(n^{2}W)$$. 

