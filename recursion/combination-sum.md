#### Combination Sum 

> Given a **set** of candidate numbers \(`candidates`\) **\(without duplicates\)** and a target number \(`target`\), find all unique combinations in `candidates` where the candidate numbers sums to `target`.
>
> The **same** repeated number may be chosen from `candidates` unlimited number of times.
>
> **Note:**
>
> * All numbers \(including `target`\) will be positive integers.
> * The solution set must not contain duplicate combinations.
>
> **Example 1:**
>
> ```
> Input: candidates = [2,3,6,7], target = 7,
> A solution set is:
>
> [
>   [7],
>   [2,2,3]
> ]
>
> ```
>
> **Example 2:**
>
> ```
> Input: candidates = [2,3,5], target = 8,
> A solution set is:
>
> [
>   [2,2,2,2],
>   [2,3,3],
>   [3,5]
> ]
>
> ```

##### Backtracking:

```py
def combinationSum(candidates: List[int], target: int) -> List[List[int]]:
    
    def helper(start, cur_seq, target, candidates, res):
        if target == 0:
            res.append(cur_seq[:])
            return
        if target < 0:
            return
        for i in range(start, len(candidates)):
            if candidates[i] > target:
                break
            cur_seq.append(candidates[i])
            helper(i, cur_seq, target - candidates[i], candidates, res)
            cur_seq.pop()
        
    res = []
    candidates.sort()
    helper(0, [], target, candidates, res)
    return res
```

This is a classic backtracking problem. We first sort the array in order to prune our search as soon as the current candidate is greater than our target \(all numbers after will be at least as big as the current one so no point in continuing\). 

The key to not containing duplicate combinations is to pass a `start` variable to our helper function so we don't use numbers previously used. This ensures that our combinations are always in increasing order, so once we have a combination like $\small <2,3,3>$, we won't have something like $\small <3,2,3>$.

