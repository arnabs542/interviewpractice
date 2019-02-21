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

The above approach is a bit similar to dynamic programming. Suppose we have all the subsets of a $$\small n$$ element array. If we introduce a $$\small n+1$$ element, then to generate the additional subsets, we add the new element to all the existing subsets and combine that with all the existing subsets which don't include the new element. 

We treat duplicate element as a special element. For example, if we have duplicate elements \(5, 5\), instead of treating them as two elements that are duplicate, we can treat it as one special element 5, but this element has more than two choices: you can either NOT put it into the subset, or put ONE 5 into the subset, or put TWO 5s into the subset. 

We first put the array into the counter, and then build our power set iteratively. For each element, the existing subsets are the subsets that don't include the element. To generate all subsets that contain the element, we loop through the number of times the element appears, and add each iteration to all existing subsets. 

For example, suppose our array was `[1,2,2]`. We first begin with `res = [[]]`. We have one 1, so our power set is now `res = [[], [1]]`. We have two 2s, which means we can not put any 2s into our subsets, which is covered by the existing power set. We can also put one 2 into all existing subsets, giving us `res = [[], [1], [2], [1,2]]`. Lastly, we can put two 2s into all subsets, giving us `res = [[], [1], [2], [1,2], [2,2], [1,2,2]]`.

