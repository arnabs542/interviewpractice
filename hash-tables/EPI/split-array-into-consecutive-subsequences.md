#### Split Array into Consecutive Subsequences

> You are given an integer array sorted in ascending order \(may contain duplicates\), you need to split them into several subsequences, where each subsequences consist of at least 3 consecutive integers. Return whether you can make such a split.
>
> **Example 1:**
>
> ```
> Input: [1,2,3,3,4,5]
>
> Output: True
>
> Explanation:
> You can split them into two consecutive subsequences : 
> 1, 2, 3
> 3, 4, 5
> ```
>
> **Example 2:**
>
> ```
> Input: [1,2,3,3,4,4,5,5]
>
> Output: True
>
> Explanation:
> You can split them into two consecutive subsequences : 
> 1, 2, 3, 4, 5
> 3, 4, 5
> ```
>
> **Example 3:**
>
> ```
> Input: [1,2,3,4,4,5]
>
> Output: False
> ```

##### Hash table \(Chains\):

```py
def isPossible(nums):

    count = collections.Counter(nums)
    tails = collections.defaultdict(int)

    for num in nums:

        if count[num] == 0:
            continue

        if tails[num] > 0:
            tails[num] -= 1
            tails[num + 1] += 1

        elif count[num + 1] > 0 and count[num + 2] > 0:                
            count[num + 1] -= 1
            count[num + 2] -= 1
            tails[num + 3] += 1

        else:
            return False

        count[num] -= 1

    return True
```

The idea here is to maintain a hash table of the current chains we have built. The entry `count[x]` records how many chains ended at `x-1`; in other words, how many chains are currently "waiting" for `x`.

This approach is greedy by nature - we consistently try to extend each chain to its maximum length. We only start a new chain when we can't attach the current number to any existing chain.

Runtime and space complexity are both $\small \mathcal O(n)$. 

