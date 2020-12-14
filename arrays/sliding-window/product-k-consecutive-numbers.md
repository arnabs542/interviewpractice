#### Product of K Consecutive Numbers

> Given an array of integers nums of size `n`, and an int `k`. Return an array result of size `n` such that `result[i] = nums[max(0, i-k+1)] * ... * nums[i]`. In other words, each element is the product formed from a sliding window of size `k`, possibly cut short by the left side of the array.
>
> Example:
> ```
> Input: nums = [1, 3, 3, 6, 5, 7, 0, -3], k = 3
> Output: [1, 3, 9, 54, 90, 210, 0, 0]
> Explanation: 1 (1), 3 (1x3), 9 (1x3x3), 54 (3x3x6), 90 (3x6x5), 210 > (6x5x7), 0 (5x7x0), 0 (7x0x-3)
>```

##### Solution 

This problem is mainly to test our ability to detect edge cases. We can start by simply keeping a rolling product, and deleting items as the go out of range:

```py
def product_k_consecutive(num: "List[int]", k: "int") -> "List[int]":
    res = []
    product = 1
    for i in range(len(nums)):
        product *= nums[i]
        if i >= k:
            product /= nums[i-k]
        res.append(product)
    return product
```

However, this approach completely breaks upon encountering `0`'s. Not only can we not divide by `0`, but once we multiply a `0`, we completely lose the rest of the window. For example, suppose we are given an input `nums = [1,0,3,4,5], k = 3`. After the `0` goes out of range, we have no idea what our product should be now. 

The brute force way to solve this would be use a nested for loop to only update items within the window. This takes $\small \mathcal O(n^{2})$ time. A more subtle solution requires us to only see how many `0`'s are in the current window and deal with them specifically:

```py
def product_k_consecutive(num: "List[int]", k: "int") -> "List[int]":
    res = []
    product = 1
    zero_count = 0
    for i in range(len(nums)):
        if nums[i] == 0:
            zero_count += 1
        else:
            product *= nums[i]
        if i >= k:
            if nums[i-k] == 0:
                zero_count -= 1
            else:
                product /= nums[i-k]
        res.append(product if not zero_count else 0)
    return product
```

When we encounter an `0` in the current window, we simply increment our `0` counter. We a `0` goes out of scope, we decrement our zero counter. This allows us to preserve the non-zero portion of the window for use later. Running time is $\small \mathcal O(n)$.