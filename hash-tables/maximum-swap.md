#### Maximum Swap

> Given a non-negative integer, you could swap two digits at most once to get the maximum valued number. Return the maximum valued number you could get.
>
> Example 1:
> ```
> Input: 2736
> Output: 7236
> Explanation: Swap the number 2 and the number 7.
> ```
> Example 2:
>```
> Input: 9973
> Output: 9973
> Explanation: No swap.
>```
> Note: The given number is in the range [0, 108]

##### Solution

The problem essentially boils to two parts:
1. What is the furtherest left number in the prefix that has a larger number in the suffix?
2. Of the numbers in suffix greater than the prefix number, what is the largest number, and where is it's last apperance?

There are multiple ways to tackle this problem, but the simplest is to use a hash map to record the last apperance of each digit. We iterate through the number from left to right, and each time we look for the largest number in the suffix greater than it. If the last apperance is later than the index of the current number, we replace it and we're done. 

```py
def maximumSwap(num: int) -> int:

    A = list(map(int, str(num)))
    last = {x: i for i, x in enumerate(A)}


    for i, x in enumerate(A):
        for d in range(9, x, -1):
            if last.get(d, -1) > i:
                A[i], A[last[d]] = A[last[d]], A[i]
                return int("".join(map(str, A)))

    return num
```

Time and space complexity are $\small \mathcal O(n)$.