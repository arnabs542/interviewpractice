Enumerate Score Combinations



Backtracking:

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



