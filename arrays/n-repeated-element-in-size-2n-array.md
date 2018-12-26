#### N-Repeated Element in Size 2N Array

> In a array `A` of size `2N`, there are `N+1` unique elements, and exactly one of these elements is repeated N times.
>
> Return the element repeated `N` times.
>
> **Example 1:**
>
> ```
> Input: 
> [1,2,3,3]
> Output: 
> 3
> ```
>
> **Example 2:**
>
> ```
> Input: 
> [2,1,2,5,3,2]
> Output: 
> 2
> ```
>
> **Example 3:**
>
> ```
> Input: 
> [5,1,5,2,5,3,5,4]
> Output: 
> 5
> ```

##### Set:

```py
def repeatedNTimes(A):
    nums = set()
    for e in A:
        if e in nums:
            return e
        nums.add(e)
```

The easiest solution would be to simply allocate some extra memory to store all the numbers seen so far. Return as soon as we come across a repeat. 

Time and space complexity are both $$\small \mathcal O(n)$$. 

##### Pigeonhole:

```py
def repeatedNTimes(A):
    N = len(A)

    for i in range(N):
        if A[i] == A[i-1] or A[i] == A[i-2]:
            return A[i]
```



