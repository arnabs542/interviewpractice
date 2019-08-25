#### Increasing Triplet Subsequence - LC 334

> Given an unsorted array return whether an increasing subsequence of length 3 exists or not in the array.
>
> Formally the function should:
>
> * Return true if there exists i, j, k
>    such that arr\[i\] &lt; arr\[j\] &lt; arr\[k\] given 0 ≤ i &lt; j &lt; k ≤ n-1, else
> * Return false
>
> **Note:** Your algorithm should run in O\(n\) time complexity and O\(1\) space complexity.
>
> **Example 1:**
>
> ```
> Input: [1,2,3,4,5]
> Output: true
> ```
>
> **Example 2:**
>
> ```
> Input: [5,4,3,2,1]
>Output: false
> ```

##### LIS:

```py
def increasingTriplet(nums: "List[int]") -> bool:
    a = b = None
    for n in nums:
        if a is None or n <= a:
            a = n
        elif b is None or n <= b:
            b = n
        else:
            return True
    return False
```

This problem is essentially asking if there exists a strictly increasing subsequence with at least 3 elements. We use two variables `a` and `b` to keep track of the subsequence. When we first encounter an element or an element smaller than the smallest one we have, we set `a` equal to that element. If that element is strictly larger than `a`, but `b` is none or larger than that element, we set `b` equal to that element. If that element is strictly larger than `b`, we return `True`, since we have such a subsequence.

It's important that we set elements equal to `a` and `b` as well, since we need to ensure that `a < b`. For example, given an array like `[1,2,1,2,1,2]`, if we use `n < a` as the comparison, we'll end up with `a = b = 1` after the second `1`, and the second `2` will cause us to erroneously return `True`.

