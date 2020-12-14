#### Split Array Largest Sum

> Given an array which consists of non-negative integers and an integer m, you can split the array into m non-empty continuous subarrays. Write an algorithm to minimize the largest sum among these m subarrays.
>
> **Note:**  
>  If n is the length of array, assume the following constraints are satisfied:
>
> * 1 ≤ n ≤ 1000
> * 1 ≤ m ≤ min\(50, n\)
>
> **Examples:**
>
> ```
> Input: nums = [7,2,5,10,8], m = 2
>
> Output:
> 18
>
> Explanation:
> There are four ways to split nums into two subarrays. The best way is to split it into [7,2,5] and [10,8], 
> where the largest sum among the two subarrays is only 18.
> ```

##### Code:

```py
def splitArray(self, nums, m):

    # Test if solution is viable
    def can_split(nums, splits, max_ele):
        acc = 0
        for e in nums:
            if acc + e <= max_ele:
                acc += e
            else:
                splits -= 1
                if splits < 0:
                    return False
                acc = e
        return True

    lo, hi = float("-inf"), 0
    for e in nums:
        lo = max(lo, e)
        hi += e

    while lo < hi:
        mid = (lo + hi) >> 1
        if can_split(nums, m - 1, mid):
            hi = mid
        else:
            lo = mid + 1
    return lo
```

##### Explanation:

This problem is not immediately clear that it is a trial-and-error problem. We first need to establish the search space, which can be determined by the input _m_. Instead of thinking of _m_ as the number of subarrays, we'll instead think of it as the number of cuts we are given. Suppose that we are given no cuts. Then the subarray is the entire input array we are given, and the smallest largest sum would be the sum of the entire array. On the other hand, suppose we were given an infinite number of cuts. Then the smallest largest sum would be largest element in the array. And thus, we have our search space: `[max(nums), sum(nums)]`.

Next is to figure out how to verify our guess efficiently. To do so we simply try to cut our array in accordance with the guess we made. We try to pack as much as we can into each subarray before we make a split, since this will allow us to conserve the splits most optimally. We return False when we come across a situation that we need to make a split and we have no split left, or when we encounter an element that is indivually larger than the upper bound we picked \(although this shouldn't be possible in this case since our lower bound is the largest single element\).

Running time is bounded by $$\small \mathcal O(n \log{s})$$, where $$\small s$$ is the sum of the total array.

