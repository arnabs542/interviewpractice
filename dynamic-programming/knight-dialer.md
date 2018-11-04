#### Knight Dialer

> A chess knight can move as indicated in the chess diagram below:
>
> ![](/assets/knight_dialer.png)
>
> This time, we place our chess knight on any numbered key of a phone pad \(indicated above\), and the knight makes `N-1` hops.  Each hop must be from one key to another numbered key.
>
> Each time it lands on a key \(including the initial placement of the knight\), it presses the number of that key, pressing `N` digits total.
>
> How many distinct numbers can you dial in this manner?
>
> Since the answer may be large, **output the answer modulo **`10^9 + 7`.
>
> **Example 1:**
>
> ```
> Input: 1
> Output: 10
> ```
>
> **Example 2:**
>
> ```
> Input: 2
> Output: 20
> ```
>
> **Example 3:**
>
> ```
> Input: 3
> Output: 46
> ```

##### Brute Force:

My first attempt at solving this problem was to brute force generate all possible numbers of length N:

```
def knightDialer(N):
    press = {
        0 : [4,6],
        1 : [6,8],
        2 : [7,9],
        3 : [4,8],
        4 : [3,9,0],
        5 : [],
        6 : [1,7,0],
        7 : [2,6],
        8 : [1,3],
        9 : [2,4]            
    }

    dials = set()
    length = N

    def helper(key, N, cur, dials):
        if N == 0:
            if len(cur) == length:
                dials.add("".join(map(lambda x: str(x), cur)))
            return
        for k in press[key]:
            cur.append(k)
            helper(k, N - 1, cur, dials)
            cur.pop()

    for i in range(10):
        helper(i, N-1, [i], dials)

    return len(dials) % (10 ** 9 + 7)
```

However, this quickly blew up in terms of time complexity. A rough analysis of the time complexity can be seen that the helper function calls itself at most 3 times \(keys 4 and 6 have 3 possible moves\) which requiring a call stack of depth of N. The recurrence formula assumes some shape similar to $$\small T(N) = 3*T(N-1) + O(1)$$, which solves to some form of $$\small \mathcal O(3^{N})$$.

##### Dynamic Programming



