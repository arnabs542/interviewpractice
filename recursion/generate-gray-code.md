#### Generate Gray Code

> The gray code is a binary numeral system where two successive values differ in only one bit.
>
> Given a non-negative integer _n_ representing the total number of bits in the code, print the sequence of gray code. A gray code sequence must begin with 0.
>
> **Example 1:**
>
> ```
> Input: 2
>
> Output: [0,1,3,2]
> Explanation:
>
> 00 - 0
> 01 - 1
> 11 - 3
> 10 - 2
>
> For a given n, a gray code sequence may not be uniquely defined.
> For example, [0,2,3,1] is also a valid gray code sequence.
>
> 00 - 0
> 10 - 2
> 11 - 3
> 01 - 1
> ```
>
> **Example 2:**
>
> ```
> Input: 0
>
> Output:[0]
>
> Explanation: We define the gray code sequence to begin with 0. A gray code sequence of n has size = 2^n, which for 
> n = 0 the size is 2^0 = 1. Therefore, for n = 0 the gray code sequence is [0].
> ```

##### Backtracking \(Brute Force\):

```py
def grayCode(n: "int") -> "List[int]":

    def differ_by_one(x, y):
        xor = x ^ y
        count = 0
        while xor:
            count += xor & 1
            if count > 1:
                return False
            xor >>= 1
        return count == 1

    num_range = 2**n
    code = [0]
    nums = set([i for i in range(1, num_range)])

    def helper(code, nums):
        if len(nums) == 0:
            return differ_by_one(code[-1], code[0])
        for i in nums:
            if differ_by_one(i, code[-1]):
                code.append(i)
                nums.discard(i)
                if helper(code, nums):
                    return code
                code.pop()
                nums.add(i)

    helper(code, nums)
    return code
```

My initial approach was to generate a set of all values for the sequence, then try to insert each of them if they differ from the previous insertion by one bit. Since there are $$\small 2^{n}$$ numbers, and a total of $$\small (2^{n})!$$ permutations, the runtime is $$\small \mathcal O((2^{n})!)$$, which is incredibly slow.

##### Backtracking \(Reverse and Add\):

```py
def grayCode(n: "int") -> "List[int]":
    if n == 0:
        return [0]
    code = grayCode(n-1)
    return code + [(1 << (n-1)) + e for e in reversed(code)]
```



