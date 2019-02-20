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

##### Recursion \(No Loop\):

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

The runtime for the above algorithm is $$\small \mathcal O(2^{n})$$, since for every element, we can choose to include it or exclude it from our current set. Therefor, we have a total of $$\small 2^{n}$$ decisions to make.

Space is bounded by $$\sum_{i=0}^{n} \binom{n}{n-i} * (n-i) \small {= 2^{n-1}*n = \mathcal O(2^{n})}$$

##### Recursion \(Loop\):

```py
def generate_power_set(S):
    power_set = []

    def helper(i, cur, S):
        for j in range(i, len(S)):
            cur.append(S[j])
            helper(j+1, cur, S)
            cur.pop()
        power_set.append(cur.copy())

    helper(0, [], S)
    return power_set
```



