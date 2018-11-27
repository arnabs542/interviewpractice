#### Bag of Tokens

> You have an initial power `P`, an initial score of `0` points, and a bag of tokens.
>
> Each token can be used at most once, has a value `token[i]`, and has potentially two ways to use it.
>
> * If we have at least `token[i]` power, we may play the token face up, losing `token[i]` power, and gaining `1` point.
> * If we have at least `1` point, we may play the token face down, gaining `token[i]` power, and losing `1` point.
>
> Return the largest number of points we can have after playing any number of tokens.
>
> **Example 1:**
>
> ```
> Input: tokens = [100], P = 50
> Output: 0
> ```
>
> **Example 2:**
>
> ```
> Input: tokens = [100,200], P = 150
> Output: 1
> ```
>
> **Example 3:**
>
> ```
> Input: tokens = [100,200,300,400], P = 200
> Output: 2
> ```

##### Code \(Brute Force - Timed Out\):

```py
def bagOfTokensScore(tokens, P):

    max_points = 0

    def helper(tokens, P, points):
        nonlocal max_points
        max_points = max(max_points, points)
        for i,t in enumerate(tokens):
            if t != 'U' and t != 'D' and P >= t:    # try playing up to gain a point
                tokens[i] = 'U'
                helper(tokens, P - t, points + 1)
                tokens[i] = t
            if points > 0 and t != 'D' and t != 'U':  # try playing down to gain P
                tokens[i] = 'D'
                helper(tokens, P + t, points - 1)
                tokens[i] = t

    helper(tokens, P, 0)
    return max_points
```

We can simply try to brute force all possible moves: 1\) if we have enough points, we can try to get more power, 2\) if we have enough power, we can try to get more points. Since there are 2 

