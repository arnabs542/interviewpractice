#### Group Anagrams

> Given an array of strings, group anagrams together.
>
> **Example:**
>
> ```
> Input:
> ["eat", "tea", "tan", "ate", "nat", "bat"]
> ,
>
> Output:
>
> [
>   ["ate","eat","tea"],
>   ["nat","tan"],
>   ["bat"]
> ]
> ```

The above problem is a classic problem where a hash table can provide an efficient algorithm. The key idea is to map strings to a representative. Given any string, the sorted version can be used as a unique identifier for the anagram group it belongs to, since all anagrams must have not only the same set of characters, but also the same count for each of those characters.

##### Code \(Sorting\):

```py
def groupAnagrams(strs):
    groups = collections.defaultdict(list)
    for s in strs:
        groups[("".join(sorted(s)))].append(s)
    return [group for group in groups.values()]
```

Overall runtime is $$\small \mathcal O(nm \log{m})$$, where $$\small m$$ is the length of the longest string, and $$\small n$$ is the number of strings in the array. 

In fact, we can actually improve the running time by implementing a hashing function of our own. We map each letter to a prime number, and we construct the hash of the string by multiplying all the characters as their prime forms together. We use prime numbers so the product is unique to the string. 

##### Code \(Hashing\):

```
def groupAnagrams(strs):

    primes = [2, 41, 37, 47, 3, 67, 71, 23, 5, 101, 61, 17, 19, 13, 31, 43, 97, 29, 11, 7, 73, 83, 79, 89, 59, 53]

    def hash(string):
        res = 1
        for c in string:
            res *= primes[ord(c) - ord('a')]
        return res

    groups = collections.defaultdict(list)
    for s in strs:
        groups[hash(s)].append(s)
    return [group for group in groups.values()]
```

This will bring the time complexity down to $$\small O(mn)$$. However, this ignores the details in how multiplication is done.

