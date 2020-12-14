#### Rearrange String k Distance Apart

> Given a non-empty string **str** and an integer **k**, rearrange the string such that the same characters are at least distance **k** from each other.
>
> All input strings are given in lowercase letters. If it is not possible to rearrange the string, return an empty string `""`.
>
> **Example 1:**
>
> ```
> str = "aabbcc", k = 3
>
> Result: "abcabc"
>
> Explanation: The same letters are at least distance 3 from each other.
> ```
>
> **Example 2:**  
>
>
> ```
> str = "aaabc", k = 3 
>
> Answer: ""
>
> Explanation: It is not possible to rearrange the string.
> ```
>
> **Example 3:**  
>
>
> ```
> str = "aaadbbcc", k = 2
>
> Answer: "abacabcd"
>
> Another possible answer is: "abcabcda"
>
> Explanation: The same letters are at least distance 2 from each other.
> ```

My original approach was to create an array of a length equal to the given string, since the lengths must be the same. We then use a heap to keep track of which letters have the highest frequency, and track to put those distance _k_ apart, starting from the first empty index. However, this fails for the input of "aabbcc" with k = 2, since our string will end up like "abab\_\_" by the time we get to "c", and our algorithm will declare it impossible. 

##### Code:

```py
def rearrangeString(string, k):

    cnts = [0] * 26;
    for c in string:
        cnts[ord(c) - ord('a')] += 1

    sorted_cnts = []
    for i in range(26):
        sorted_cnts.append((cnts[i], chr(i + ord('a'))))
    sorted_cnts.sort(reverse=True)

    max_cnt = sorted_cnts[0][0]
    blocks = [[] for _ in range(max_cnt)]
    i = 0
    for cnt in sorted_cnts:
        for _ in range(cnt[0]):
            blocks[i].append(cnt[1])
            i = (i + 1) % max(cnt[0], max_cnt - 1)

    for i in range(max_cnt-1):
        if len(blocks[i]) < k:
            return ""

    return "".join(map(lambda x : "".join(x), blocks))
```

##### Explanation:

We still begin by sorting the characters with respect to their frequency, since we want to deal with the most frequent elements first. However, instead of trying to fit everything into the final array, we now deal with the characters by putting them in blocks. The idea is this: suppose we were given the string "aaabbc", _k_ = 2. We then know that our final answer, if possible, must be in the form of "axxaxxaxx", since the "a" characters must be at least 2 apart. We can then think of each "axx" as a block, or a bucket. Our final answer will thus be composed of 3 buckets, 2 of which must be of length _k_. 

We begin by dealing with the elements relative to their frequency. For each element that appears `max_cnt` number of times, we just distribute them into every frame. For elements that appear less than the maximum count, we will linearly distribute into every frame except the last one, because that is the only frame that is not required to have a length of _k_. In the end, we simply loop through every bucket, up to the last one, and ensure that they all have a length of at least _k _- otherwise the elements between the buckets will be closer than _k_ distance apart. 

