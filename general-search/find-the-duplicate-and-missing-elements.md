#### Find the Duplicate and Missing Elements

> Given an array of $$\small n$$ integers, each between $$\small 0$$ and $$\small n - 1$$ inclusive, eactly one element appears twice, implying that exactly one number between $$\small 0$$ and $$\small n - 1$$ is missing from the array. Find the missing and duplicate numbers.

An easy solution would be to sort the array, then iterate through until we find the duplicate numbers. This takes $$\small \mathcal O(n \log(n))$$ time, but it modifies the array, which might not be allowed. Another option is to use extra space, such as a hash table or a set, and this would bring time complexity to $$\small \mathcal O(n)$$. There is actually an $$\small \mathcal O(n)$$ time complexity and $$\small \mathcal O(1)$$ space complexity solution.

##### Bit Manipulation Code:

```py
def find_duplicate_missing(A):

    miss_XOR_dup = functools.reduce(lambda v, i: v ^ i[0] ^ i[1], enumerate(A), 0)

    diff_bit, miss_or_dup = miss_XOR_dup & (~(miss_XOR_dup - 1)), 0
    for i, a in enumerate(A):
        if i & diff_bit:
            miss_or_dup ^= i
        if a & diff_bit:
            miss_or_dup ^= a

    return DuplicateAndMissing(miss_or_dup, miss_or_dup ^ miss_XOR_dup) if miss_or_dup in A \
        else DuplicateAndMissing(miss_or_dup ^ miss_XOR_dup, miss_or_dup)
```

##### Explanation:

This problem is a more advanced version of the classical problem where we are asked to find the duplicate in an array where all elements appear once except one which appears twice. In that problem, we simply needed to XOR all elements together, since$$\small x \oplus x = 0$$.

The first thing to notice is that we are given very strict bounds on the range of possible numbers. Specifically, the range of possible values of the elements is also the range of the length of the array. This means that all elements appear twice, except the duplicate which appears three times and the missing which appears once. We begin by iterating through the array, XORing all elements with the index number. This leaves us with `miss_XOR_dup`, which is the missing and duplicate values XOR'd together. 

We now need to find a bit in `miss_XOR_dup` which is 1, since that bit is unique to either the missing or the duplicate value \(if the two values both have that bit, it would be a 0\). Now we iterate through the array a second time, XORing all indices and values with that bit set. The result will either be the missing or the duplicate number. Why? Suppose that it is the missing number with the 1 at that bit. All other numbers with that bit will appear twice: once in the vector, and once in the index. The missing number will be the only number to appear once, and will therefore be the only one left. On the other hand, suppose it is the duplicate number with the 1. Then all other numbers will appear twice, and the duplicate number will appear 3 times. 

We can now figure out whether `miss_or_dup` is the missing or duplicate number with a third and final pass through the array.

##### Bonus:

> Given an array nums containing n + 1 integers where each integer is between 1 and n \(inclusive\), prove that at least one duplicate number must exist. Assume that there is only one duplicate number, find the duplicate one.
>
> **Example 1:**
>
> ```
> Input:
> [1,3,4,2,2]
> Output: 2
> ```
>
> **Example 2:**
>
> ```
> Input:
>  [3,1,3,4,2]
>
> Output: 3
> ```
>
> **Note:**
>
> 1. You **must not** modify the array \(assume the array is read only\).
> 2. You must use only constant, O\(1\) extra space.
> 3. There is only one duplicate number in the array, but it could be repeated more than once.

In this case, the above code may not work, depending on how many times the duplicated number is repeated. Instead, we can use the _Floyd Cycle Detection _algorithm, also known as _Floyd's Tortoise and Hare _algorithm. 

Normally this is used for linked lists, but because of the unique constraints of the array elements, we can essentially treat our array as a linked list, where each index is a node, and each value in the node is a pointer to the next node. 

##### Code:

```py
def findDuplicate(self, nums):

    slow = fast = 0
    slow, fast = nums[slow], nums[nums[fast]]

    # Find a node in the cycle
    while slow != fast:
        slow, fast = nums[slow], nums[nums[fast]]

    # Find head of cycle
    fast = 0
    while slow != fast:
        fast, slow = nums[fast], nums[slow]

    return slow
```

A few things to note here:

1. Since there are $$\small n+1$$ numbers, and elements start at 1, we are guaranteed to not start immediately on a cycle, since  `nums[0] != 0`

1. `slow` and `fast` point to the array indices \("nodes"\), and not the actual values. 



