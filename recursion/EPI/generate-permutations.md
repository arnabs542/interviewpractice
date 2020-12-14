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

##### Backtracking \(Set\):

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

There are $\small \mathcal O(n!)$ possible permutations, and at the end of each permutation we need to copy the array. The overall runtime is bounded by $\small \mathcal O(n * n!)$.

The above solution uses $\small \mathcal O(n)$ space in addition to the recursion stack, due to the set being used to stored chosen elements.

##### Backtracking \(Swap\):

```py
def permutations(A):
    res = []

    def directed_perm(i):
        if i == len(A) - 1:
            res.append(A.copy())
        for j in range(i, len(A)):
            A[i], A[j] = A[j], A[i]
            directed_perm(i+1)
            A[i], A[j] = A[j], A[i]

    directed_perm(0)
    return res
```

The time complexity of the above solution doesn't change, but we save on space because we don't use a set to store chosen elements anymore. 

The idea of the above algorithm is as follows: suppose we are given an input array $\small <7,3,5>$. We would first generate all permutations with 7, which requires us to generate all permutations of $\small <3,5>$. We then find all permutations beginning with 3. Since $\small <5>$ is a single element, we just return it. This implies $\small <3,5>$ has a single permutation with 3. Next we look for permutations of $\small <3,5>$ beginning with 5. To do this, we swap 3 and 5, and find, as before, there is a single permutation of $\small <3,5>$, namely $\small <5,3>$. Hence, there are two permutations of $\small <7,3,5>$ beginning with 7. We then swap 7 with 3 to find all permutations with 3, and then finally swap 7 with 5 to find all permutations starting with 5.

