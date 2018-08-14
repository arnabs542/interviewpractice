

#### Koko Eating Bananas

> Koko loves to eat bananas.  There are `N` piles of bananas, the `i`-th pile has `piles[i]` bananas.  The guards have gone and will come back in `H` hours.
>
> Koko can decide her bananas-per-hour eating speed of `K`.  Each hour, she chooses some pile of bananas, and eats K bananas from that pile.  If the pile has less than `K` bananas, she eats all of them instead, and won't eat any more bananas during this hour.
>
> Koko likes to eat slowly, but still wants to finish eating all the bananas before the guards come back.
>
> Return the minimum integer `K` such that she can eat all the bananas within `H` hours.
>
> **Example 1:**
>
> ```
> Input: piles = [3,6,7,11], H = 8
> Output: 4
> ```
>
> **Example 2:**
>
> ```
> Input: piles = [30,11,23,4,20], H = 5
> Output: 30
> ```
>
> **Example 3:**
>
> ```
> Input: piles = [30,11,23,4,20], H = 6
> Output: 23
> ```
>
> **Note:**
>
> * `1 <= piles.length <= 10^4`
> * `piles.length <= H <= 10^9`
> * `1 <= piles[i] <= 10^9`

##### Code:

```py
def minEatingSpeed(self, piles, H):

    def can_finish(piles, speed):
        return sum(map(lambda x: math.ceil(x/speed), piles)) <= H

    lo, hi = 1, pow(10, 9)
    while lo < hi:
        mid = (lo + hi) >> 1
        if can_finish(piles, mid):
            hi = mid
        else:
            lo = mid + 1

    return lo
```

##### Explanation:

This problem is a perfect example of trial and error, since it fulfills both conditions.

We are able to establish a search space, since we are told that `1 <= piles[i] <= 10^9`. We are able to efficiently traverse this space, because given a speed _s_, if _s_ is not able to satisfy the time constraint, then we know that there is no point in checking smaller numbers. Similarly if _s_ satisfies the constraint, we don't need to check numbers higher than it either. 

Verifying the candidate takes $$\small \mathcal O(n)$$ time, brushing aside the intricacies of doing division and addition. 



