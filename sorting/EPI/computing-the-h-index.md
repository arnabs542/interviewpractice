#### Computing the H-Index

> Given an array of citations \(each citation is a non-negative integer\) of a researcher, write a function to compute the researcher's h-index.
>
> According to the [definition of h-index on Wikipedia](https://en.wikipedia.org/wiki/H-index): "A scientist has index `h` if `h` of his/her `N` papers have **at least** `h` citations each, and the other `N − h` papers have **no more than** `h` citations each."
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

##### Solution:

My initial mistake was assuming that the h-index must be a value within the citation array - this is not necessarily the case. If citations = `[100]`, then the h-index = `1`, since there is `1` paper with at least `1` citation.

First it is helpful to rephrase the problem in a clearer way. Let the function `count(citations, h)` return the number of papers with citations equal to or greater than h. The h-index of an author can then be simply understood as the greatest integer h for which `count(citations, h) >= h`. This means that the index must be bounded between `[0, len(citations)]`. We could not possibly have an h-index greater than the number of numbers, since `count(citations, h) <= len(citations)`.

The brute force approach to solving this problem would be simply count up from 0 until we reach the first number in which the number of papers with citations greater than equal to it is less than it. However, this takes $\small \mathcal O(n^{2})$ time.

However, if we sort the index, we can find `count(citations, h)` in constant time. Let's look at the below example:

```
idx:        0 1 2 3 4 5 6 7  8
citations:  0 3 3 4 4 6 6 12 15
count:      9 8 7 6 5 4 3 2  1
```
As we previouly established, the h-idx lies in `[0, n]`, where `n = len(citations)`. We can now simply start at n and work backwards until we find a valid h-idx.

We begin by guessing the h-idx is `n`. However, we see this isn't possible, because in order for h-idx to be `9`, the number of citations at `citations[0]` must be equal to/greater than `9`. But the citation of the first paper is only `0`. We then try for an h-idx of `8`. Again, we see it's not possible because the second peper doesn't have enough. When we get to an h-idx of `4`, we see its possible, because there are `4` papers with citations >= `4`. Thus our h-idx is `4`.

In other words, if a paper contributes to the h-idx, all later papers (in a sorted array) will do so as well.

The first improvement we can make is realizing that if `count(citations, a) == b`, then if `count(citations, c) == d`, and `c >= a`, then `b >= d`. This makes sense, since suppose we have `4` papers each with at least `3` citations, then if we look for the number of papers with at least `4` citations, we can not have more than `4` papers meet that requirement. The next realization is that if we were to sort the citations, we can compute `count(citations, c)` in constant time, since the number of papers with citations $\geq \small c$ is simply the length of the array minus the index of $\small c$.

```py
def h_index(citations):
    citations.sort()
    n = len(citations)
    for i, c in enumerate(citations):
        if c >= n - i:
            return n - i
    return 0
```

We begin by sorting the citations and then advancing through the sorted array. If we come across a citation which is greater than equal to the number of papers left, then we stop, because the h-index requires **at least** h papers containing h citations each. Because the citations are sorted, once we find a citation $\small c$ greater than equal to the number of papers left, we know all later citations will be greater than equal to $\small c$, meaning that `count(citations, d) <= count(citations, c)`, for all `d >= c`.



