## Minimum Adjacent Swaps for K Consecutive Ones

> You are given an integer array, nums, and an integer k. nums comprises of only 0's and 1's. In one move, you can choose two adjacent indices and swap their values.
>
> Return the minimum number of moves required so that nums has k consecutive 1's.
>
> Example 1:
>```
>Input: nums = [1,0,0,1,0,1], k = 2
>Output: 1
>Explanation: In 1 move, nums could be [1,0,0,0,1,1] and have 2 consecutive 1's.
>```
>Example 2:
>```
>Input: nums = [1,0,0,0,0,0,1,1], k = 3
>Output: 5
>Explanation: In 5 moves, the leftmost 1 can be shifted right until nums >= [0,0,0,0,0,1,1,1].
>```
>Example 3:
>```
>Input: nums = [1,1,0,1], k = 2
>Output: 0
>Explanation: nums already has 2 consecutive 1's.
>```
> Constraints:
>
>    - 1 <= nums.length <= 105
>    - nums[i] is 0 or 1.
>    - 1 <= k <= sum(nums)

This problem is quite difficult to even solve via brute force. It's not trivial to figure out the possible search space, and how to even build each one and test it. 

The first key trick to this problem is realizing that we only care about the `1`'s. More specifically, we only care about the indices of the `1`'s. Since our end goal is to make them consecutive, then we can discard the `0`s from the array.

Once we record down only the indices of the `1`'s, we can see that this lends itself to a sliding window problem. For example, given the array `[1,0,0,1,0,1]`, after transforming it we get `[0,3,5]`. Now, we only need to consider each window of size `k`, and figure out the smallest cost for making the elements consecutive. 

It turns out that moving elements to the median is the best approach (we can find proofs online). In the above example, since `k` is an even number, we can choose any of the two medians and it'll be fine. So for the first window, we have `[0,3]`. We need to essentially transform it to `[0,1]`.

A common trick to calculate the cost of making this consecutive is to first move everything to one location then slacken the constraint. For example, given the array `[0,3,6,8,12]`, the cost of transforming it to `[4,5,6,7,8]` is the same as moving everything to `6`, then subtracting the extra moves. For example, we only needed to move `0` to `4`, so we essentially have `6 - 0 + 2 = 4`. From this, we can see that extra moves are actually symmetric about the median. Let's say we have `r_l` elements on the left and `r_r` elements on the right. Then we have two summations from `1` to either `r_l` or `r_r` that tells us how much we need to subtract. 

If we try to compute the cost for each sliding window we will time out. Thus the second key trick is to realize that we can build each window off of the previous one. Let's look at the example `[1,4,5,7,9,10,11,12,14]`, with `k = 7`. The first window is: `[1,4,5,7,9,10,11]`, and the cost to make everything consecutive is `8`. The second window is `[4,5,7,9,10,11,12]`. There's only four changes we need to make to update for each window:
1. Drop the left-most element 
2. Update the costs of the remaining left elements to move to the new median
3. Update the costs of the remaining right elements to move to the new median
4. Add the cost from the new right-most element

We don't actually need to worry about the slack, because that will apply to every single window, since we always move everything to the median first before reverting them back to their proper places. 

```py
def minMoves(nums: List[int], k: int) -> int:
    ones = [i for i, e in enumerate(nums) if e]
        
    # Calculate the cost for the first window - O(k)
    cur = 0
    r_l = (k-1) // 2    # Number of elements on the left
    r_r = k // 2        # Number of elements on the right
    mid = (k-1) // 2    # Index of median

    # Calculate cost to move everything to the median
    for i in range(k):
        cur += abs(ones[i] - ones[mid])

    # Subtract the slack since we moved too far
    cur -= (r_l**2 + r_l) // 2 + (r_r**2 + r_r) // 2
    best = cur

    for i in range(k, len(ones)):
        # Remove leftmost element
        cur -= ones[mid] - ones[mid-r_l]    

        # Diff between new mid and prev mid
        diff = ones[mid+1] - ones[mid]        

        # Remaining left elements (including old median) must move 
        # more to new median
        cur += r_l * (diff)
        # Remaining right elements should move less to new meidan
        cur -= (r_r) * (diff)

        # Update mid
        mid += 1

        # Newest right-most number in the window
        cur += ones[mid + r_r] - ones[mid]

        # Check for value
        best = min(best, cur)

    return best
```
Time and space complexity are both $O(n)$.


