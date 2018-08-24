#### Find Smallest Subarray Sequentially Covering all Values

> In the previous problem, we did not differentiate between the order in which keywords appeared. If the digest has to include the keywords in the order in which they appear in the search textbox, we may get a different digest. Write a program that takes two arrays of strings, and return the indices of the starting and ending index of a shortest subarray of the first array \(the "paragraph" array\) that "sequentially covers", i.e., contains all the strings in the second array \(the "keywords" array\), in the order in which they appear in the keywords array. Assume keywords are distinct.
>
> Example:
>
> ```
> paragraph = <"apple", "banana", "cat", "apple">
> keywords = <"banana", "apple">
>
> answer: [1,3]
> ```
>
> Explanation:
>
> The paragraph subarray starting at index 0 and ending at index 1 does not fulfill the specification, even though it conatins all the keywords, since they do not appear in the specified order. On the other hand, the subarray starting at index 1 and ending at index 3 does fulfill the specification.

My first attempt was to simply modify the code from the previous problem:

```py
def find_smallest_sequentially_covering_subset(paragraph, keywords):
    word_count = collections.Counter(keywords)
    left = cur_word_idx = 0
    res = (-1, -1)

    for right, word in enumerate(paragraph):
        if word in word_count:
            word_count[word] -= 1
        if word == keywords[cur_word_idx]:
            cur_word_idx += 1

        while cur_word_idx == len(keywords):
            if res == (-1, -1) or right - left < res[1] - res[0]:
                res = (left, right)
            left_word = paragraph[left]
            if left_word in word_count:
                word_count[left_word] += 1
                if word_count[left_word] > 0:
                    cur_word_idx = keywords.index(left_word)
            left += 1

    return Subarray(res[0], res[1])
```

We still use a hash table to keep track of frequency of keyword occurences, however we key focal point is the index of the current keyword we are looking for. When we have found all keywords, we then start moving the left boundary inwards until we lose all occurrences of keyword, in which case we start moving right again looking for that keyword. However, this doesn't work in the situation where later examples of earlier keywords occur. For example, take the paragraph array: \[O, X, Y, R, O, Z, M, P\] and the keyword array \[O, R, M\]. We sill begin the while loop after reaching the M character, but when we move our left boundary past the first O, we need to start over, else the sequence is broken. However, because we have an extra O in the subarray, the left boundary will stop after moving past the first R, since that is the first keyword to be depleted.

##### Code:

```py
def find_smallest_sequentially_covering_subset(paragraph, keywords):

    keyword_to_idx = {k: i for i, k in enumerate(keywords)}
    latest_occurence = [-1] * len(keywords)
    shortest_subarray_length = [float("inf")] * len(keywords)

    shortest_distance = float("inf")
    result = Subarray(-1, -1)

    for i, p in enumerate(paragraph):
        if p in keyword_to_idx:
            keyword_idx = keyword_to_idx[p]
            if keyword_idx == 0:
                shortest_subarray_length[keyword_idx] = 1
            elif shortest_subarray_length[keyword_idx - 1] < float("inf"):
                distance_to_prev_keyword = i - latest_occurence[keyword_idx - 1]
                shortest_subarray_length[keyword_idx] = distance_to_prev_keyword + shortest_subarray_length[keyword_idx - 1]
            latest_occurence[keyword_idx] = i

            if keyword_idx == len(keywords) - 1 and shortest_distance > shortest_subarray_length[-1]:
                shortest_distance = shortest_subarray_length[-1]
                result = Subarray(i - shortest_distance + 1, i)

    return result
```

##### Explanation:

The key to avoid doing work 

