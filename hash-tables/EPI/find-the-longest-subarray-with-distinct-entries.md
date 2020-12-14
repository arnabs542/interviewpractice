#### Find the Longest Subarray with Distinct Entries

> Write a program that takes an array and returns the length of a longest subarray with the property that all its elements are distinct. For example, if the array is $\small <f,s,f,e,t,w,e,n,w,e,>$ then a longest subarray all of whose elements are distinct is $\small <s,f,e,t,w>$.

##### Code:

```py
def longest_subarray_with_distinct_entries(A):
    start = 0
    seen_elements = {}
    longest = 0
    for i, a in enumerate(A):
        if a in seen_elements and seen_elements[a] >= start:
            start = seen_elements[a] + 1
        seen_elements[a] = i
        longest = max(longest, i - start + 1)
    return longest
```

##### Explanation:

The key idea here is that we use a hash map to constantly store when the last occurrence of an entry was seen, so we can update our subarray in $\small \mathcal O(1)$ time. This is a two pointer problem; we keep advancing the right pointer until we reach an element seen before that is within our current subarray. To make the subarray valid again, we simply need to start at 1 past the index at which the element appears in, since it is the first duplicate we come across. Within each iteration, we update the length until we reach the end. 

