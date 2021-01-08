## Minimum Swaps to Make Strings Equal

>You are given two strings s1 and s2 of equal length consisting of letters "x" and "y" only. Your task is to make these two strings equal to each other. You can swap any two characters that belong to different strings, which means: swap s1[i] and s2[j].
>
>Return the minimum number of swaps required to make s1 and s2 equal, or return -1 if it is impossible to do so.
> 
>
>Example 1:
>```
>Input: s1 = "xx", s2 = "yy"
>Output: 1
>Explanation: 
>Swap s1[0] and s2[1], s1 = "yx", s2 = "yx".
>```
>Example 2: 
>```
>Input: s1 = "xy", s2 = "yx"
>Output: 2
>Explanation: 
>Swap s1[0] and s2[0], s1 = "yy", s2 = "xx".
>Swap s1[0] and s2[1], s1 = "xy", s2 = "xy".
>Note that you can't swap s1[0] and s1[1] to make s1 equal to "yx", cause >we can only swap chars in different strings.
>```
>Example 3:
>```
>Input: s1 = "xx", s2 = "xy"
>Output: -1
>```
>Example 4:
>```
>Input: s1 = "xxyyxyxyxx", s2 = "xyyxyxxxyx"
>Output: 4
>```
> 
>
>Constraints:
>
>  - 1 <= s1.length, s2.length <= 1000
>  - s1, s2 only contain 'x' or 'y'.

### Greedy

The key to solving this problem is recognizing the patterns. Since we're not asked to explicitly list out the swaps, only figure out the number of swaps, we should realize that we don't need to explicitly manage the two strings. 

For the following example, let's iterate through both strings:

|     index    | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
|:------------:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| letter of s1 | x | x | y | y | x | y | x | y | x | x |
| letter of s2 | x | y | y | x | y | x | x | x | y | x |

- At position `0`, both characters are the same so let's skip it
- At position `1`, we have `s1[1] = x` and `s2[1] = y`

This is a mismatch, so we need to think about how we can fix it. If we find another mismatch where `s1[i] = x` and `s2[i] = y` then we can fix it with just one swap. If we find a mismatch where `s1[i] = y` and `s2[i] = x` then we can fix it with two swaps. 

This right here is the key to solving this problem. Go through both strings and look for mismatches, and record the count of each type of mismatch. At the end, we need to check if we have enough types of each mismatch to be able to make the two strings equal. We obviously want to match each type with itself first, because that requires only one swap. Otherwise, we have to try to match it with the other type, which results in 2 swaps. 

```py
def minimumSwap(s1: str, s2: str) -> int:
    x_y = y_x = 0
    
    for i in range(len(s1)):
        if s1[i] != s2[i]:
            if s1[i] == 'x':
                x_y += 1
            else:
                y_x += 1
    
    # We'll need an even number of mismatches to be able to fix the strings
    if (x_y + y_x) % 2:
        return -1
    
    res = x_y // 2 + y_x // 2
    x_y %= 2
    y_x %= 2
    
    res += 2 * x_y
    
    return res
```