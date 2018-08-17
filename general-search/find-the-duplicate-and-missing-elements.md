#### Find the Duplicate and Missing Elements

> Given an array of $$\small n$$ integers, each between $$\small 0$$ and $$\small n - 1$$ inclusive, eactly one element appears twice, implying that exactly one number between $$\small 0$$ and $$\small n - 1$$ is missing from the array. Find the missing and duplicate numbers.

An easy solution would be to sort the array, then iterate through until we find the duplicate numbers. This takes $$\small \mathcal O(n \log(n))$$ time, but it modifies the array, which might not be allowed. Another option is to use extra space, such as a hash table or a set, and this would bring time complexity to $$\small \mathcal O(n)$$. There are actually two solutions that have $$\small \mathcal O(n)$$ time complexity and $$\small \mathcal O(1)$$ space complexity.

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



