#### Generate Permutations II

> Given a collection of numbers that might contain duplicates, return all possible unique permutations.
>
> **Example:**
>
> ```
> Input: [1,1,2]
>
> Output:
> [
>   [1,1,2],
>   [1,2,1],
>   [2,1,1]
> ]
> ```

##### Backtracking \(Key + Set\):

```py
def permuteUnique(nums: 'List[int]') -> 'List[List[int]]':
    perm_set = set()
    perms = []
    
    def directed_perm(i):
        if i == len(nums):
            key = "".join(map(lambda x: str(x), nums))
            if key in perm_set:
                return
            perms.append(nums.copy())
            perm_set.add(key)
            return
        for j in range(i, len(nums)):
            nums[i], nums[j] = nums[j], nums[i]
            directed_perm(i+1)
            nums[i], nums[j] = nums[j], nums[i]
    
    directed_perm(0)
    return perms
```

Running time of the algorithm is $\small \mathcal O(n*n!)$. We use the same method as the parent problem to generate the permutations. Once we generate a permutation, we convert the array to a string to check if this particular permutation has already been generated. 

Storing the keys requires $\small \mathcal O(n!)$ space.

