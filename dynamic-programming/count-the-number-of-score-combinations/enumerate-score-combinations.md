#### Enumerate Score Combinations

> Write a program that takes a final score and scores for individual plays, and returns the number of sequences of plays that result in the final score. For example, 18 sequences of plays yield a score of 12. Some examples are $$\small <2,2,2,3,3>, <2,3,2,2,3>,<2,3,7>,<7,3,2>$$.

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

This problem is extremely similar to the Combination Sum problem on Leetcode. The main difference here is that ordering matters, i.e. $$\small <2,3,7>$$ and $$\small <7,3,2>$$ count as different sequences. 

To enumerate all sequences, we simply employ backtracking. We loop through the array, and we try to pick each number. If the current number brings us over the final score, we get rid of it and return. Otherwise, we keep it and move on to the next score. 

In the Leetcode problem, we avoided repeating sequences by passing in a starting index to the helper function - suppose we picked the 2nd element for our current score, then the next element has to start at an index of at least 2. 

