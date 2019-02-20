#### Generate the Power Set

> Given a set of **distinct** integers, _nums_, return all possible subsets \(the power set\).
>
> **Note:** The solution set must not contain duplicate subsets.
>
> **Example:**
>
> ```
> Input: nums = [1,2,3]
>
> Output:
> [
>   [3],
>   [1],
>   [2],
>   [1,2,3],
>   [1,3],
>   [2,3],
>   [1,2],
>   []
> ]
> ```

##### Recursion:

```py
def generate_power_set(S):
    power_set = []

    def helper(idx, cur, S):
        if idx == len(S):
            power_set.append(cur.copy())
            return
        # Include current element
        cur.append(S[idx])
        helper(idx+1, cur, S)
        cur.pop()
        # Exclude current element
        helper(idx+1, cur, S)

    helper(0, [], S)
    return power_set
```

The runtime for the above algorithm is $$\small \mathcal O(n*2^{n})$$, since for every element, we can choose to include it or exclude it from our current set. Therefore, we have a total of $$\small 2^{n}$$ decisions to make. After calculating each powerset, we need to copy the current array into our final array, resulting in the additional $$\small n$$ factor.

Space is bounded by $$\sum_{i=0}^{n} \binom{n}{n-i} * (n-i) \small { \, = 2^{n-1}*n = \mathcal O(2^{n})}$$. Another way to look at this is that we have $$\small 2^{n}$$ elements, and on average each element has $$\small {n}/{2}$$ elements within.

##### Recursion \(Bit Mask\):

```py
def generate_power_set(S):
    power_set = []

    for int_for_subset in range(1 << len(S)):
        bit_array = int_for_subset
        subset = []
        while bit_array:
            subset.append(int(math.log2(bit_array & ~(bit_array - 1))))
            bit_array &= bit_array - 1
        power_set.append(subset)
    return power_set
```

For a given ordering of the elements of $$\small S$$, there exists a one-to-one correspondence between the $$\small 2^{n}$$ bit arrays of length $$\small n$$ and the set of all subsets of $$\small S$$ - the 1s in the $$\small n$$-length bit array $$\small v$$ indicate  the elements of $$\small S$$ in the subset corresponding to $$\small v$$. For example, suppose $$\small S = \{a,b,c,d\}$$. The bit array $$\small <1,0,1,1>$$ corresponds to the subset $$\small \{a,c,d\}$$

