#### Test for Palindromic Permutations

> A palindrom is a string that reads the same forwards and backwards, e.g., "level", "rotator", and "foobaraboof".
>
> Write a program to test whether the letters forming a string can be permuted to form a plaindrome. For example, "edified" can be permuted to form "deified".

##### Code:

```py
def can_form_palindrome(s):
    return sum (v % 2 for v in collections.Counter(s).values()) <= 1
```

##### Explanation:

A string can only be a palindrom if and only if the number of characters whose frequencies is odd is at most 1. We simply count how many times each letter appears, and test if the previous condition holds. 

Time complexity: $$\small \mathcal O(n)$$, where $$\small n$$ is the length of the string. Space complexity is $$\small \mathcal O(c)$$, where $$\small c$$ is the number of unique characters. 

