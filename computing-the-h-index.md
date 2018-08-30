#### Computing the H-Index

> Given an array of citations \(each citation is a non-negative integer\) of a researcher, write a function to compute the researcher's h-index.
>
> According to the [definition of h-index on Wikipedia](https://en.wikipedia.org/wiki/H-index): "A scientist has index h if h of his/her N papers have **at least **h citations each, and the other N − h papers have **no more than**h citations each."
>
> **Example:**
>
> ```
> Input: citations = [3,0,6,1,5]
> Output: 3 
>
> Explanation: [3,0,6,1,5] means the researcher has 5 papers in total and each of them had received 3, 0, 6, 1, 5
>  citations respectively. Since the researcher has 3 papers with at least 3 citations each and the remaining two 
>  with no more than 3 citations each, her h-index is 3.
> ```

##### Code \(Sorting\):

```py
def h_index(citations):
    citations.sort()
    n = len(citations)
    for i, c in enumerate(citations):
        if c >= n - i:
            return n - i
    return 0
```

##### Explanation:

My initial mistake was assuming that the h-index must be a value within the citation array - this is not necessarily the case. If citations = \[100\], then the h-index = 1, since there is 1 paper with at least 1 citation.

First it is helpful to rephrase the problem in a clearer way. The h-index of an author is simply an integer h such that 

`h+1 >len([x for x in citations if x >= h])`

The brute force approach to solving this problem would be simply count up from 0 until we reach the first number in which the number of papers with citations greater than equal to it is less than it. However, this takes $$\small \mathcal O(n^{2})$$ time. 

We begin by sorting the citations and then advancing through the sorted array. If we come across a citation which is greater than equal to the number of papers left, then we stop, because the h-index requires **at least** h papers containing h citations each. Because the citations are sorted, once we find a citation greater than equal to the number of papers left, we know all later citations will be greater than equal to the current citations, 



Once we find a citation that is greater than equal to the number of papers left, 

