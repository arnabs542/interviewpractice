#### Find the Nearest Repeated Entries in an Array

People do not like reading text in which a word is used multiple times in a short paragraph. You are to write a program which helps identify such a problem. 

Write a program which takes as input an array and finds the distance between a closest pair of equal entries. For example, if _s _= &lt;"All", "work", "and", "no", "play", "makes", "for", "no", "work", "no", "fun", "and", "no", "results"&gt;, then the second and third occurrences of "no" is the closest pair. 

##### Code:

```py
def find_nearest_repetition(paragraph):
    closest = float("inf")
    word_idx = {}
    for i, word in enumerate(paragraph):
        if word in word_idx:
            closest = min(closest, i - word_idx[word])
        word_idx[word] = i
    return closest if closest < float("inf") else -1
```

##### Explanation:

A very direct application of using a hash table. Simply record the latest position of each word as we encounter them, and if we have already seen them before, see if this pair is closer than the previous existing pair. 



