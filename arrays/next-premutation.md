#### Next Permutation

> Implement **next permutation**, which rearranges numbers into the lexicographically next greater permutation of numbers.
>
> If such arrangement is not possible, it must rearrange it as the lowest possible order \(ie, sorted in ascending order\).
>
> The replacement must be [**in-place**](http://en.wikipedia.org/wiki/In-place_algorithm) and use only constant extra memory.
>
> Here are some examples. Inputs are in the left-hand column and its corresponding outputs are in the right-hand column.
>
> `1,2,3` → `1,3,2`  
> `3,2,1` → `1,2,3`  
> `1,1,5` → `1,5,1`

##### Unoptimized:

```py
def nextPermutation(nums):   

    ip, n = -1, len(nums)

    for i in range(1, n):
        if nums[i] > nums[i-1]:
            ip = i-1

    # Already max size
    if ip == -1:
        nums.reverse()
        return

    for i in reversed(range(ip, n)):
        if nums[i] > nums[ip]:
            nums[i], nums[ip] = nums[ip], nums[i]
            break

    nums[:] = nums[:ip+1] + list(sorted(nums[ip+1:]))
    return
```

We first approach the problem by looking at what condition\(s\) must be met such that there exists a next permutation. For example, suppose the given array was $\small [6,5,4,3,2,1]$. In this case, all elements are in descending order, and the number is at its largest.

For there to be a next permutation, there must exist an inversion point where the entry $\small e$ is smaller than some entry $\small s$ after it; e.g.: $\small [6,2,5,1,3,0]$. In this example, we see that there are a few situations where this apply: $\small 2$ and $\small 1$ both have elements larger than it come after it. But because we're looking for the next permutation, that means we need to make the smallest change possible. In that event, we need to find the right-most inversion point, and replace with the smallest element out of all elements larger than it to the right. In the above example, we need to swap the $\small 1$ and $\small 3$: $\small [6,2,5,3,1,0]$. However, this is not quite finished yet, because the suffix after the inversion point is the smallest it can be: the $\small 1$ and $\small0$ need to be flipped. We generate the smallest suffix by sorting that part of the array.

##### Optimized:

```py
def nextPermutation(nums):   

    # Find the first entry from the right that is smaller than the entry
    # immediately after it.

    ip = len(nums) - 2     
    while ip >= 0 and nums[ip] >= nums[ip+1]:
        ip -= 1

    # Already max size
    if ip == -1:
        nums.reverse()
        return

    # Swap the smallest entry after index inversion_point that is greater than
    # perm[inversion_point]. Since entries in perm are decreasing after
    # inversion_point, if we search in reverse order, the first entry that is
    # greater than perm[inversion_point] is the entry to swap with.

    for i in reversed(range(ip, len(nums))):
        if nums[i] > nums[ip]:
            nums[i], nums[ip] = nums[ip], nums[i]
            break

    # Entries in perm must appear in decreasing order after inversion_point,
    # so we simply reverse these entries to get the smallest dictionary order.
    nums[ip+1:] = reversed(nums[ip+1:])
    return
```

We can actually simply reverse the suffix to generate the smallest version instead of sorting it. The reason is this - by swapping the inversion point element with the smallest element in the suffix that's greater than it, we maintain descending order. By swapping $\small s$ and $\small e$, order is maintained, because $\small nums[idx(s)+1]$ must be less than or equal to $\small e$, else it would've gotten swapped. Therefore, descending ordering is preserved, and we only need to reverse the array.

To summarize:

1. For there to be a next permutation, there must exist at least 1 inversion point

2. The suffix to be right of the inversion point must be in sorted descending order, else the inversion point would be found among the suffix

3. By swapping the inversion point element with the smallest element in the suffix that's greater than it, we maintain descending order. By swapping s and e, order is maintained, because s + 1 must be less than or equal to e, else it would've gotten swapped.

4. We can reverse the suffix to achieve the smallest suffix.



