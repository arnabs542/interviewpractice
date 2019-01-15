#### Subarray Sums Divisible by K

> Given an array `A` of integers, return the number of \(contiguous, non-empty\) subarrays that have a sum divisible by `K`.
>
> **Example 1:**
>
> ```
> Input: 
> A = 
> [4,5,0,-2,-3,1]
> , K = 
> 5
> Output: 
> 7
> Explanation: 
> There are 7 subarrays with a sum divisible by K = 5:
> [4, 5, 0, -2, -3, 1], [5], [5, 0], [5, 0, -2, -3], [0], [0, -2, -3], [-2, -3]
> ```

##### Brute Force:

```py
def subarraysDivByK(A, K):
    """
    :type A: List[int]
    :type K: int
    :rtype: int
    """
    count = 0
    for i in range(len(A)):
        cur_sum = 0
        for j in range(i, len(A)):
            cur_sum += A[j]
            count += (cur_sum % K == 0)

    return count
```

The brute force solution is to simply use a nested for-loop to calculate all subarray sums and test their modularity. This takes $$\small \mathcal O(n^{2})$$ time and times out.

##### Prefix Sum + Hashmap

```py
def subarraysDivByK(A, K):

    sums = collections.defaultdict(int)
    cur_sum = 0
    res = 0

    for c in A:
        cur_sum += c
        if cur_sum % K == 0:
            res += 1
        for k in sums:
            if (cur_sum - k) % K == 0:
                res += sums[k]
        sums[cur_sum] += 1

    return res
```

My next solution was to use a hashmap to store all prefix sums. As the current sum increases, I loop through the recorded sums and see how many can be subtracted from the current sum resulting in subarrays divisible by $$\small K$$. However, if all prefix sums end up different, the time required for this algorithm is still $$\small \mathcal O(n^{2})$$.

##### Prefix Sums and Counting

```py
def subarraysDivByK(A, K):
    cnt = [0] * K
    cnt[0] = 1
    cur = 0
    for a in A:
        cur += a
        cnt[cur % K] += 1
    return sum(c * (c - 1) // 2 for c in cnt)
```

As is typical with problems involving subarrays, we use prefix sums to add each subarray. Let `P[i+1] = A[0] + A[1] + ... + A[i]`. Then, each subarray can be written as `P[j] - P[i]` \(for `j > i`\). Thus, we have `P[j] - P[i]` equal to `0` modulo `K`, or equivalently `P[i]` and `P[j]` are the same value modulo `K`.

Number theory portion in more detail:

We need to find all prefix sum pairs `(i,j)` such that `(P[j] - P[i]) % K == 0`. But this is only true, **if and only if**`P[j] % K == P[i] % K`.

According the the division algorithm we can express `P[j]` and `P[i]` in the following way:  
` P[j] = bK + r0 where 0 <= r0 < K`_  
_` P[i] = aK + r1 where 0<= r1 < K`

Then `P[j] - P[i] = (b*K + r0) - (a*K + r1)`  
`= b*K - a*K + r0 - r1 = K*(b-a) + r0 - r1`  
 Again: `P[j] - P[i] = K*(b-a) + (r0-r1)`, in other words, `K` only `divides P[j] - P[i]` iff `r0-r1 = 0 <-> r0 = r1`

