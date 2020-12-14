#### Longest Alternating Subsequence

> Define a sequence of numbers $\small <a_{0},a_{1},...,a_{n-1}>$to be _alternating _if $\small a_{i} < a_{i+1}$ for even $\small i$ and $\small a_{i} > a_{i+1}$ for odd $\small i$. Given an array of numbers $\small A$ of length $\small n$, find a longest subsequence $\small <i_{0},...,i_{k-1}>$ such that $\small <A[i_{0}],A[i_{1}],...,A[i_{k-1}]>$ is alternating.

##### Dynamic Programming:

```py
def longest_alternating(A):
    if not A:
        return 0
    size = [1] * len(A)
    for i in range(1, len(A)):
        for j in range(i):
            if A[i] > A[j] and size[j] % 2:
                size[i] = max(size[i], size[j] + 1)
            elif A[i] < A[j] and size[j] % 2 == 0:
                size[i] = max(size[i], size[j] + 1)
    
    return max(size)
```

The idea is essentially the same as the parent problem. Only difference now is that we need to make sure we're attaching larger numbers at even indices and smaller numbers at odd indices. 

Running time is $\small \mathcal O(n^{2})$, space is $\small \mathcal O(n)$.

