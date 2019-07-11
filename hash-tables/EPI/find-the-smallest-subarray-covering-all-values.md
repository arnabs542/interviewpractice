#### Find the Smallest Subarray Covering All Values/Minimum Window Substring

> Write a program which takes an array of strings and a set of strings, and return the indices of the start and ending index of a shortest subarray of the given array that "covers" the set, i.e., contains all strings in the set.

##### Code:

```py
def find_smallest_sequentially_covering_subset(paragraph, keywords):
    keywords_to_cover = collections.Counter(keywords)
    result = Subarray(-1, -1)
    remaining_to_cover = len(keywords)
    left = 0
    for right, p in enumerate(paragraph):
        if p in keywords:
            keywords_to_cover[p] -= 1
            if keywords_to_cover[p] >= 0:
                remaining_to_cover -= 1

        while remaining_to_cover == 0:
            if result == (-1, -1) or right - left < result[1] - result[0]:
                result = (left, right)
            pl = paragraph[left]
            if pl in keywords:
                keywords_to_cover[pl] += 1
                if keywords_to_cover[pl] > 0:
                    remaining_to_cover += 1
            left += 1
    return result
```

##### Explanation:

First let us think of simpler problems. Suppose our task was simply to figure out whether or not a subarray existed that could cover all keywords. We would initialize a counter of the keywords and iterate through the subarray, decrementing the counter each time we find a word that's the keyword. When all of the keywords have a count of 0, then we return true. If we reach the end of the paragraph and not all keywords have been found, then we return false. 

The next step would be return the indices of the subarray if it exists. The only change we need to make to the above code is we remember the first index where we found a keyword and the index where we found the last remaining keyword. 

To find the smallest subarray to cover all keywords is then a simple change of the above solution. We now utilize a sliding window method: advance the right index until we have found all keyword. Then advance the left index forwards until we no longer have all the keywords in the subarray. Then advance the right index. We do this until we finish iterating through the array. 

This algorithm takes $$\small \mathcal O(n)$$ time and space. One tip for optimization to remember is that we don't need a hash set to remember which words are left; instead, we just initialize an integer counter equal to the length of the keywords array and check its value instead. 

