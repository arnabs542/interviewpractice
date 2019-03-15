#### Compute the Binomial Coefficients

> The symbol$$\binom{n}{k}$$ is the short form for the expressions $$\frac{n(n-1)...(n-k+1)}{k(k-1)...(3)(2)(1)}$$. It is the number of ways to choose a $$\small k-$$element subset from an $$\small n-$$element set. It is not obvious that the expression defining $$\small \binom{n}{k}$$ always yields an integer. Furthermore, direct computation of $$\small \binom{n}{k}$$ from this expression quickly results in the numerator or denominator overflowing if integer types are used, even if the final result fits in a 32-bit integer. If floats are used, the expression may not yield a 32-bit integer.
>
> Design an efficient algorithm for computing $$\binom{n}{k}$$ which has the property that it never overflows if the final result fits in the integer word size.

##### Dynamic Programming:

```py
def compute_binomial_coefficient(n, k):
    if n == k or k == 0:
        return 1
    if n == 0:
        return 0
    if k == 1:
        return n

    dp = [[0] * (k+1) for _ in range(n+1)]

    for i in range(n+1):
        dp[i][1] = i
        dp[i][0] = 1

    for i in range(1, n+1):
        for j in range(2, min(i+1, k+1)):
            if j == i:
                dp[i][j] = 1
            else:
                dp[i][j] += dp[i-1][j-1] + dp[i-1][j]

    return dp[-1][-1]
```

My first instinct was try to factor $$\binom{n}{k}$$ in terms of common factors in the numerator and denominator; however that approach is unsatisfactory because the problem of factoring numbers itself is rather challenging. 

A better approach is to consider what the binomial coefficient means fundamentally. Consider the $$\small n$$th element in the initial set. A subset of size $$\small k$$ either contains this element, or doesn't. This is the basis for a recursive enumeration - find all subsets of size $$\small k-1$$ amongst the first $$\small n-1$$ elements and add the $$\small n$$th element into those sets, and then find all subsets of size $$\small k$$ amongst the first $$\small n-1$$ elements. The union of these two subsets if all subsets of size $$\small k$$. Thus, we have our recursive sequence: 
$$
\binom{n}{k} = \binom{n-1}{k} + \binom{n-1}{k-1}
$$
The base cases are $$\binom{r}{r}$$ and $$\binom{r}{0}$$, both of which are 1. The individual results from the subcalls are 32-bit integers and if $$\binom{n}{k}$$ can b represented by a 32-bit integer, they can too, so it's not possible for intermediate results to overflow.

