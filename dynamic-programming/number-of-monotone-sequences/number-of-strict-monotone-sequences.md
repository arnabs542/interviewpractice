#### Number of Strict Monotone Sequences

> Call a decimal number $$\small D$$, as defined above, strictly monotone if $$\small D[i] < D[i+1], 0 \leq i < |{D}|$$. Write a program which takes as input a positive integer $$\small k$$ and computes the number of decimal numbers of length $$\small k$$ that are strictly monotone.

Recursion -&gt; Dynamic Programming:

```py
def monotoneSequence(k):
	
	def helper(k, nums):
		if k == 0 or not nums:
			return 0
		if k == 1:
			return nums
		res = 0
		for i in range(nums):
			res += helper(k-1, nums - i - 1)
		return res
	
	return helper(k, 9)
```



