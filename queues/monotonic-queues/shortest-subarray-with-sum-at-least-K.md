#### Shortest Subarray with Sum at Least K

> Return the length of the shortest, non-empty, contiguous subarray of `A` with sum at least `K`.
> 
> If there is no non-empty subarray with sum at least `K`, return `-1`.
> 
> Example 1:
> ```
> Input: A = [1], K = 1
> Output: 1
> ```
> 
> Example 2:
> ```
> Input: A = [1,2], K = 4
> Output: -1
>```
> Example 3:
> ```
> Input: A = [2,-1,2], K = 3
> Output: 3
>```

#### Solution

This problem deals with subarray sums, so we should think about trying to use a prefix sum array to help us, since we can find out the sum of a subarray `A[i:j]` in constant time. Let `P[i] = sum(A[0:i])`. We wanted the smallest `y-x` such that `y > x` and `P[y] - P[x] >= K`.

Motivated by that equation, let `opt(y)` be the largest `x` such that `P[x] <= P[y] - K`. We need two key observations:

* If `x1 < x2` and `P[x2] <= P[x1]`, then `opt(y)` can never be `x1`, since `P[x1] <= P[y] - K` dictates that `P[x2] <= P[x1] <= P[y] - K`. Both `(x1, y)` and `(x2, y)` fulfill the sum requirement, but since `y - x2` is smaller, we can discard `x1`. This observation implies that our candidates `x` for `opt(y)` will have increasing values of `P[x]`.
* If `opt(y1) = x`, then we do not need to consider this `x` again. For if we find some `y2 > y1` with `opt(y2) = x`, then it gives us an answer of `y2 - x` which is worse (larger) than `y1 - x`.

Thus, we have our solution. We maintain a monotonic increasing queue of indices of `P`: a deque of indices `x_0`, `x_1` ... such that `P[x_0] < P[x_1] ...`. These will the possible starting indices of the subarrays. 

As we come across a new index `y`, we'll pop from the front of the deque while `P[0] - P[x_0] >= K`. We don't need those values anymore since the subarray they form with `y` will be the smallest - all other `y`'s come later and will result in a worse subarray.

Afterwards, we get rid of all indices in the back of the array such that `P[x_n] > P[y]`. This is because since they are still in the array, they're still waiting for a later index that's at least `K` larger than them. But since 'P[y]' is just as good and appears later, it will give us the smaller sub array.

```py
def shortestSubarray(A, K):
    N = len(A)
    P = [0]
    for x in A:
        P.append(P[-1] + x)

    #Want smallest y-x with Py - Px >= K
    ans = N+1 # N+1 is impossible
    monoq = collections.deque() #opt(y) candidates, represented as indices of P
    for y, Py in enumerate(P):
        #Want opt(y) = largest x with Px <= Py - K
        while monoq and Py <= P[monoq[-1]]:
            monoq.pop()

        while monoq and Py - P[monoq[0]] >= K:
            ans = min(ans, y - monoq.popleft())

        monoq.append(y)

    return ans if ans < N+1 else -1
```

Runtime and space are both $\small \mathcal O(n)$.