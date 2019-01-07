#### Generate Permutations

> Given a collection of **distinct** integers, return all possible permutations.
>
> **Example:**
>
> ```
> Input:
>  [1,2,3]
>
> Output:
>
> [
>   [1,2,3],
>   [1,3,2],
>   [2,1,3],
>   [2,3,1],
>   [3,1,2],
>   [3,2,1]
> ]
> ```

##### Backtracking:

```py
def permute(nums):

    res = []
    used = set()

    def helper(res, used, cur):
        if len(used) == len(nums):
            res.append(cur[:])
            return 
        for i in range(len(nums)):
            if i in used:
                continue
            cur.append(nums[i])
            used.add(i)
            helper(res, used, cur)
            cur.pop()
            used.discard(i)

    helper(res, used, [])
    return res
```

This is a classic backtracking problem. At each level, we simply choose one element for the current position before calling the next level to finish the array for us. Since permutations don't allow reusing numbers, we add the index of the number we picked to a set so that for the rest of the array we won't pick it again. 

There are $$\small \mathcal O(n!)$$ possible permutations, and at the end of each permutation we need to copy the array. The overall runtime is bounded by $$\small \mathcal O(n * n!)$$.

