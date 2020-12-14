#### Combination Sum II

> Given a collection of candidate numbers \(`candidates`\) and a target number \(`target`\), find all unique combinations in `candidates` where the candidate numbers sums to `target`.
>
> Each number in `candidates` may only be used **once** in the combination.
>
> **Note:**
>
> * All numbers \(including `target`\) will be positive integers.
> * The solution set must not contain duplicate combinations.
>
> **Example 1:**
>
> ```
> Input: candidates = [10,1,2,7,6,1,5], target = 8,
> A solution set is:
>
> [
>   [1, 7],
>   [1, 2, 5],
>   [2, 6],
>   [1, 1, 6]
> ]
>
> ```
>
> **Example 2:**
>
> ```
> Input: candidates = [2,5,2,1,2], target = 5,
> A solution set is:
>
> [
>   [1,2,2],
>   [5]
> ]
> ```

##### Backtracking:

```py
def combinationSum2(candidates: List[int], target: int) -> List[List[int]]:

    def helper(start, cur_seq, target, candidates, res):
        if target == 0:
            res.append(cur_seq[:])
        if target < 0:
            return
        for i in range(start, len(candidates)):
            if i > start and candidates[i] == candidates[i-1]:
                continue
            if candidates[i] > target:
                break
            cur_seq.append(candidates[i])
            helper(i + 1, cur_seq, target - candidates[i], candidates, res)
            cur_seq.pop()
    
    res = []
    candidates.sort()
    helper(0, [], target, candidates, res)
    return res
```

The difference between this and the original problem is that the candidate numbers may contain duplicates. This means that unless we're careful in how we backtrack, we'll end up with duplicate values. 

