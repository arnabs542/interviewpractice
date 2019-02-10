#### Android Unlock Patterns

> Given an Android `3x3` key lock screen and two integers `m` and `n`, where `1 ≤ m ≤ n ≤ 9`, count the total number of unlock patterns of the Android lock screen, which consist of minimum of `m` keys and maximum `n` keys.
>
> **Rules for a valid pattern:**
>
> 1. Each pattern must connect at least `m` keys and at most `n` keys.
> 2. All the keys must be distinct.
> 3. If the line connecting two consecutive keys in the pattern passes through any other keys, the other keys must have previously selected in the pattern. No jumps through non selected key is allowed.
> 4. The order of keys used matters. 
>    ![](https://lintcode-media.s3.amazonaws.com/problem/andriod-unlock.png "android unlock")
>
> **Explanation:**
> ```
> | 1 | 2 | 3 |
> | 4 | 5 | 6 |
> | 7 | 8 | 9 |
> ```
>
> **Invalid move:**`4 - 1 - 3 - 6`  
>  Line 1 - 3 passes through key 2 which had not been selected in the pattern.
>
> **Invalid move:**`4 - 1 - 9 - 2`  
>  Line 1 - 9 passes through key 5 which had not been selected in the pattern.
>
> **Valid move:**`2 - 4 - 1 - 3 - 6`  
>  Line 1 - 3 is valid because it passes through key 2, which had been selected in the pattern
>
> **Valid move:**`6 - 5 - 4 - 1 - 9 - 2`  
>  Line 1 - 9 is valid because it passes through key 5, which had been selected in the pattern.
>
> ### Example
>
> Given m = `1`, n = `1`, return `9`.

##### DFS:

```py
    def numberOfPatterns(self, m: "int", n: "int") -> "int":
        # Initial set up
        skips = [[0] * 10 for _ in range(10)]
        skips[1][3] = skips[3][1] = 2
        skips[1][7] = skips[7][1] = 4
        skips[3][9] = skips[9][3] = 6
        skips[7][9] = skips[9][7] = 8
        skips[1][9] = skips[9][1] = skips[2][8] = skips[8][2] = skips[3][7] = skips[7][3] =\
        skips[4][6] = skips[6][4] = 5
        visited = [False] * 10
        
        def dfs(cur, remaining):
            if remaining < 0:
                return 0
            if remaining == 0:
                return 1
            visited[cur] = True
            res = 0
            for i in range(1, 10):
                if not visited[i] and (skips[cur][i] == 0 or visited[skips[cur][i]]):
                    res += dfs(i, remaining - 1)
            visited[cur] = False
            return res
        
        res = 0
        for i in range(m, n+1):
            res += dfs(1, i-1) * 4  # 1,3,7,9 are symmetric
            res += dfs(2, i-1) * 4  # 2,4,6,8 are symmetric
            res += dfs(5, i-1)
        return res
```



