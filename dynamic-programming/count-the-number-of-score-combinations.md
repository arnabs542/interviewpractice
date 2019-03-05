#### Count the Number of Score Combinations

> In an American football game, a play can lead to 2 points \(safety\), 3 points \(field goal\), or 7 points \(touchdown, assuming the extra point\). Many different combinations of 2,3, and 7 point plays can make up a final score. For example, four combinations of plays yield a score of 12:
>
> * 6 safeties \(2\*6 = 12\)
> * 3 safeties and 2 field goals \(2\*3 + 3\*2 = 12\)
> * 1 safety, 1 field goal and 1 touchdown \(2\*1 + 3\*1 + 7\*1 = 12\), and 
> * 4 field goals \(3\*4 = 12\)
>
> Write a program that takes the final score and scores for individual plays, and returns of the number of combinations of plays that result in the final score.

##### Bottom Up \(Incorrect\):

```py
def num_combinations_for_final_score(final_score, individual_play_scores):
    # One way to reach 0
    combos = [1] + [0]*final_score
    for i in range(final_score+1):
        for s in individual_play_scores:
            if s <= i:
                combos[i] += combos[i-s]
    return combos[-1]
```

My original solution was based on the Coin Change problem above. However, this leads to counting duplicates. For example, suppose the score is 5, and the individual plays are \[1,2\]. The dp array will look like this:

| 0 | 1 | 2 | 3 | 4 | 5 |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | 1 | 2 | 3 | 5 | 8 |

For score = 3, there's only two ways to get it: \(1,2\), \(1,1,1\). However, we ended up counting \(1,2\) and \(2,1\) as two separate combinations. We get a combination of \(1,2\) by doing `combo[3] += combo[3-2]`. We get a combination of \(2,1\) by doing `combo[3] = combo[3-1]`, since 2 is a score by itself, so it contributes to the number of ways we get a score of 2.

The fundamental difference between this problem and the Coin Change problem is that in the Coin Change problem, we were simply concerned with finding the smallest combination. Thus, it didn't matter if we process \(1,2\) and \(2,1\) - all we keep is their length. In this problem, we need to figure out how many combinations there are, which means we need to avoid double counting.

##### Bottom Up \(By play\):

```py
def num_combinations_for_final_score(final_score, individual_play_scores):
    # One way to reach 0
    combos = [1] + [0]*final_score
    for x in individual_play_scores:
        for i in range(x, final_score+1):
            combos[i] += combos[i - x]
    return combos[-1]
```

Suppose the score is 5, and the individual plays are worth \[1,2\]. The dp array will look like this after each outer iteration:

```
# x = 1
[1, 1, 1, 1, 1, 1]

# x = 2
[1, 1, 2, 2, 3, 3]
```

The reason we loop the scores in the outer loop is because we're interested in the total number of combinations, not permutations. If the outer loop is the amount, then the same combination will be counted multiple times because they can come in different orders. By setting the outer loop as the scores, we assure that for any valid combination, the order of each score will always be the same as their order in `individual_play_scores`, so there can be no duplicates.

Time complexity is bounded by $$\small \mathcal O(n*s)$$, where $$\small n,s$$ represent the final score and the number of individual play scores. Space is bounded by $$\small \mathcal O(n)$$.

