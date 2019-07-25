#### K Empty Slots

> There is a garden with `N` slots. In each slot, there is a flower. The `N` flowers will bloom one by one in `N` days. In each day, there will be `exactly` one flower blooming and it will be in the status of blooming since then.
>
> Given an array `flowers` consists of number from `1` to `N`. Each number in the array represents the place where the flower will open in that day.
>
> For example, `flowers[i] = x` means that the unique flower that blooms at day `i` will be at position `x`, where `i` and `x` will be in the range from `1` to `N`.
>
> Also given an integer `k`, you need to output in which day there exists two flowers in the status of blooming, and also the number of flowers between them is `k` and these flowers are not blooming.
>
> If there isn't such day, output -1.
>
> Example:
>
> Given flowers = `[1,3,2]`, k = `1`, return `2`.
>
> ```
> Explanation: 
> In the second day, the first and the third flower have become blooming.
> ```
>
> Given flowers = `[1,2,3]`, k = `1`, return `-1`.

The problem statement is a bit confusing, but the main idea is rather straightforward. We are given an array of flowers and an integer `k`. Everyday, a flower transitions from the non-blooming state to the blooming state, where it will permanently remain. The number in the `flowers` array tell us which flowers blooms on that day. For example, if `flowers[0] = 4`, then on the 0th day, the flower at position 4 blooms. We are tasked to find the earliest occurrence in which there are two flowers `k ` distance apart, and none of the flowers between the two are blooming.

##### Brute Force:

```py
def kEmptySlots(flowers, k):
    N = len(flowers)
    blooming = {}
    for i, p in enumerate(flowers):
        if p - 1 - k in blooming:   # There is a flower k positions ahead in blooming
            # check all flowers in between are 0
            if all(x not in blooming for x in range(p-k, p)):
                return i + 1
        elif p + 1 + k in blooming: # There is a flower k positions behind in blooming
            # check all flowers in between are 0
            if all(x not in blooming for x in range(p, p+1+k)):
                return i + 1
        blooming[p] = 1
    return -1
```

My first approach was to use a hash table to remember each location that blooms as we process them. We then check both ways to see if the flower in position `x - 1 - k` or position `x + 1 + k` have bloomed. If so, we then check to see if the `k` flowers between the two boundary points are blooming - if not, then return, because we have found our answer. Since running through the entire array takes $\small \mathcal O(n)$ time, and we run 2 $\small \mathcal O(k)$ checks for each element, our final algorithm is bounded by $\small \mathcal O(nk)$ time. Space complexity is bounded by $\small \mathcal O(n)$.

##### BBST (Balanced Binary Search Tree):

This is actually the most straightforward solution, as it reduces the problem to its core question. In essence, what we're asking is that for each new position that blooms, what is the closest position that bloomed before it and the closest position after it. If either of this positions are exactly `k+1` spots away, then we can return. 

A BBST is a perfect structure for this, because inserting an element and finding its predecessor/successor are all $\small \mathcal O(\log{n})$. Therefore, the total time complexity is $\small \mathcal O(n\log{n})$, since we do 3 $\small \mathcal O(\log{n})$ operations per element.

The implementation will be a bit tricking in Python, which by default has no sorted container. However, we can use C++'s set object, which is built on a balanced bst.

##### Bucketing:

```py
def kEmptySlots(flowers, k):
    bs = k + 1
    lows, highs = [float('inf')] * math.ceil(len(flowers) / bs), [float('-inf')] * math.ceil(len(flowers) / bs)
    for i in range(len(flowers)):
        x = flowers[i]
        p = (x - 1) // bs
        if x < lows[p]:
            lows[p] = x
            if p > 0 and highs[p-1] == x - k - 1:
                return i + 1
        if x > highs[p]:
            highs[p] = x
            if p < len(lows) - 1 and lows[p+1] == x + k + 1:
                return i + 1
    return -1
```

This solution is a bit tricky - the key realization is that the upper or lower bound to each position must be in a specific range. For example, suppose `k = 1`. Then if we were just told that the flower at `x = 2` bloomed, we would directly check whether the flower at `x = 0` or `x = 4` bloomed, before checking to see all flowers in between are not blooming.

We partition the array into buckets of size `k+1`, and for each flower that blooms, we check whether it is the lowest or highest position in that bucket \(if it is not either extreme, that means there is a flower on both side with distance less than `k` that is blooming, and therefore we can not possibly finish on that day\). If it is either extreme \(or both\), we then check to see if its counter part is the highest or lowest blooming position in the correct bucket. 

The bucket solution is really just an extrememly optimized way of implementing find the closest elements smaller than and greater than our new element. Unlike a BST, which will eventually end up storing all elements, we only keep what we need, which is 2 elements per bucket. 

