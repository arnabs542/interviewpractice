#### Enumerate Score Combinations

Write a program that takes a final score and scores for individual plays, and returns the number of sequences of plays that result in the final score. For example, 18 sequences of plays yield a score of 12. Some examples are $$\small <2,2,2,3,3>, <2,3,2,2,3>,<2,3,7>,<7,3,2>$$.

##### Backtracking:

```py
def combinationSum(scores, target):

    res = []

    def helper(target, scores, cur_score, res):
        if target == 0:
            res.append(cur_score[:])
            return
        if target < 0:
            return
        for i in range(len(scores)):
            if scores[i] > target:
                continue
            cur_score.append(scores[i])
            helper(i, target - scores[i], scores, cur_score, res)
            cur_score.pop()

    helper(0, target, scores, [], res)
    return res
```



