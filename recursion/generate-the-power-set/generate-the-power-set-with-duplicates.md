#### Generate the Power Set with Duplicates

> Given a collection of integers that might contain duplicates, _**nums**_, return all possible subsets \(the power set\).
>
> **Note:** The solution set must not contain duplicate subsets.
>
> **Example:**
>
> ```
> Input: [1,2,2]
>
> Output:
> [
>   [2],
>   [1],
>   [1,2,2],
>   [2,2],
>   [1,2],
>   []
> ]
> ```

##### Recursion + Key:

```py
def subsetsWithDup(nums: 'List[int]') -> 'List[List[int]]':
    set_keys = set()
    res = []

    def helper(idx, cur):
        if idx == len(nums):
            key = "".join(map(str, cur))
            if key in set_keys:
                return
            set_keys.add(key)
            res.append(cur.copy())
            return
        cur.append(nums[idx])
        helper(idx+1, cur)
        cur.pop()
        helper(idx+1, cur)

    nums.sort()
    helper(0, [])
    return res
```

We generate all subsets the same way as the parent problem. Once we have a subset, we turn its values into a string and check if we've already done a subset with the same string. If not, we add that subset to our power set and store its key.

It's important to sort the array before hand, otherwise the key comparison will not work.

Time complexity remains $$\small \mathcal O(n*2^{n})$$, and space complexity is $$\small \mathcal O(n*2^{n})$$ worst case.

##### Iteration + Hashmap:

```py
def subsetsWithDup(nums: 'List[int]') -> 'List[List[int]]':
    
    res = [[]]

    for num, freq in collections.Counter(nums).items():
        res_len = len(res)
        for i in range(1, freq+1):
            for r in res[:res_len]:
                res.append(r + [num] * i)
    return res
```



