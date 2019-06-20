##### Majority Element II

> Given an integer array of size n, find all elements that appear more than `⌊n/3⌋` times.
>
> Note: The algorithm should run in linear time and in O(1) space.
>
>  Example 1:
> ```
> Input: [3,2,3]
> Output: [3]
> ```
> Example 2:
> ```
> Input: [1,1,1,3,3,2,2,2]
> Output: [1,2]
> ```

##### Solution

The intution is this: suppose we have three different votes, and we remove them. Repeat this with triplets of different votes until there are no longer three votes left. A number originally with more than one third of the votes will still have at least one vote left, otherwise we would have hidden more than three times one third of the votes - more votes than there originally were. 

```py
def majorityElement(nums, List[int]) -> List[int]:
    ctr = collections.Counter()
    for n in nums:
        ctr[n] += 1
        if len(ctr) == k:
            ctr -= collections.Counter(set(ctr))
    ctr = collections.Counter(n for n in nums if n in ctr)
    return [n for n in ctr if ctr[n] > len(nums)/k]
```

The above algorithm is generalized to finding all elements that appear more than `⌊n/k⌋` times. 

The running time is $\small \mathcal O(N*k)$. The $\small k$ factor does not actually come from the hiding of the votes - rather, it comes from the false positive checks at the end. 

While hidding votes does take $\small \mathcal O(k)$ time, we only do this a total of $\small N/k$ times - in other words, we can't hide a vote more than once. 