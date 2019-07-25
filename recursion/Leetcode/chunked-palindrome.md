#### Chunked Palindrome

> Normal palindrome is defined as a string that reads the same backwards as forwards, for example `"abccba"`.
> Chunked palindrome is defined as a string that you can split into chunks and it will form a palindrome.
> For example, `"volvo"`. You can split it into `(vo)(l)(vo)`. Let `A = "vo"`, `B = "l"`, so the original string is `ABA` which is a palindrome.
>
> Given a string `str`, find the maximum number of chunks we can split that string to get a chuncked palindrome.
>
> Example 1:
> ```
> Input: "valve"
> Output: 1
> Explanation: You can't split it into multiple chunks so just return 1 (valve)
>```
>
> Example 2:
>```
> Input: "voabcvo"
> Output: 3
> Explanation: (vo)(abc)(vo)
>```
>
> Example 3:
>```
> Input: "vovo"
> Output: 2
> Explanation: (vo)(vo)
>```
> Example 4:
>```
> Input: "volvolvo"
> Output: 5
> Explanation: (vo)(l)(vo)(l)(vo)
>```
> Example 5:
>```
> Input: "volvol"
> Output: 2
> Explanation: (vol)(vol)
>```
> Example 6:
>```
> Input: "aaaaaa"
> Output: 6
> Explanation: We can split it into (aaa)(aaa), but the optimal split should be (a)(a)(a)(a)(a)(a)
>```

##### Solution

Since the words themselves have to the same, we just start by slicing off substrings from the beginning and end of the word until we get a match and then continue. 

```py
def find_max_palindrom_chunk(s: "str") -> int:
    if not s:
        return 0
    for i in range(len(s) >> 1):
        if s[:i+1] == s[len(s)-i-1:]:
            return 2 + find_max_palindrom_chunk(s[i+1:len(s)-i-1])
    return 1
```

Running time is $\small \mathcal O(n^2)$ due to comparing substrings and recursion. However, since strings are immutable in Python, we actually need to rebuild each substring, so running time should probably actually be $\small \mathcal O(n^{3})$