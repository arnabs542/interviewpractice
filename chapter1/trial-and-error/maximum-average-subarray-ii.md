#### Maximum Average Subarray II

> Given an array with positive and negative numbers, find the `maximum average subarray` which length should be greater or equal to given length `k`.
>
> ### Example
>
> Given nums = `[1, 12, -5, -6, 50, 3]`, k = `3`
>
> Return `15.667` // \(-6 + 50 + 3\) / 3 = 15.667
>
> ### Notice
>
> It's guaranteed that the size of the array is greater or equal to _k_.

```py
def maxAverage(self, nums, k):

    def can_find(nums, avg, k):
        acc, count = 0, 0
        for i in range(len(nums)):
            for j in range(i+k, len(nums) + 1):
                if sum(nums[i:j])/(j - i) >= avg:
                    return True
        return False


    # write your code here
    lo, hi = min(nums), max(nums)
    while not math.isclose(lo, hi):
        mid = (lo + hi) / 2  # bias mid towards high
        if lo == 5e-324 or hi == 5e-324 or mid == 5e-324:
            return 0
        if can_find(nums, mid, k):
            lo = mid
        else:
            hi = mid
    return lo
```

Since we already know this is a trial-and-error problem, let's first establish the search space. The maximum possible average is equal to the max element in the array, while the smallest possible average is equal to the min element in the array.

It's important to note the peculiarity with the number 5e-324, which causes some issues with comparisons to 0. I think this has something to do with floating representation in python but I'm not sure.

The verification portion timed out, since the above implementation is at worst $$\small \mathcal O(n^{2})$$ time.

##### Code:

```py
def maxAverage(self, nums, k):

    def can_find(nums, avg, k):
        cur_sum = min_presum = pre_sum = 0
        for i in range(k):
            cur_sum += nums[i] - avg
        if cur_sum >= 0:
            return True
        for i in range(k, len(nums)):
            cur_sum += nums[i] - avg
            pre_sum += nums[i-k] - avg
            min_presum = min(min_presum, pre_sum)
            if cur_sum >= min_presum:
                return True
        return False


    lo, hi = min(nums), max(nums)
    while not math.isclose(lo, hi):
        mid = (lo + hi) / 2  
        if lo == 5e-324 or hi == 5e-324 or mid == 5e-324:
            return 0
        if can_find(nums, mid, k):
            lo = mid
        else:
            hi = mid
    return lo
```

##### Explanation:

The main bottleneck in the first approach was that the validation took too long. In order to reduce the time complexity to $$\small \mathcal O(n)$$, we need to first compute the average differently.  Assume the average is at least $$\small K$$, there exist some $$\small i, j$$ such that $$\small (A[i] + A[i+1] + ... + A[j]) / (j - i + 1) >= K$$. We multiply both sides by \(j-i+1\), and move the right side to left, and we get $$\small (A[i] - K) + (A[i+1] - K) + ... + (A[j] - K) >= 0$$. This new method of computing the average is really the key in improving time complexity.

So, let $$\small B[i] = A[i] - K$$, we only need to find an interval `[i, j]` \(`j - i + 1 > L`\) such that $$\small B[i] + ... + B[j] >= 0$$. Now the problem is: Given an array $$\small B$$ and length $$\small L$$, we are to find an interval of maximum sum whose length is more than $$\small L$$. If the maximum sum is `>= 0`, the original average number $$\small K$$ is possible.

This problem can be solved in linear time. Let `sumB[0] = 0`, `sumB[i] = B[1] + B[2] + ... + B[i]`. For each index `i`, the max-sum interval which ended at `B[i]` is `sumB[i] - min(sumB[0], sumB[1], ..., sumB[i-L-1])`. When scanning the array with increasing `i`, we can maintain the `min(sumB[0], ..., sumB[i-L-1])` on the fly. Why do we need subtract previous intervals to find the max? Because each interval average is directly calculated by summing the difference between each element in the interval and the average, we can tell immediately if excluding a certain interval will improve our sum. For example, given the array: `[1, 12, -5, -6, 50, 3]`and the average `15`, the adjusted prefix sum array looks like this: `[-14, -17, -37, -58, -23, -35]`. We can immediately see that by subtracting parts of the subarray, we find find intervals where the average &gt;= 15. The reason for this is that suppose we have a long stretch of the numbers where all elements in that interval are less than the average. Then it would make no sense to include those unless we had to, for length reasons.

In other words, by subtracting the average from each element, we can tell how much this elements "hurts" us when we include it in our interval. The variable `min_presum` simply keeps track of which interval hurts us the most and by how much.

The overall running time can now be loosely bounded by $$\small \mathcal O(n * \log(C))$$, where $$\small C$$ is proportional to the max element in the array.

Another way to understand the verification portion is as a modified version of Kadane's algorithm. Let $$\small A$$ be the average and $$\small C_i$$ b the $$\small i^{th}$$ element of subsequence of length $$\small n$$. We essentially want to answer the question: **does there exist a subsequence with average that is **_**at least **_$$\small A$$?

$$ A &lt;= \frac{1}{n} \sum_{i=1}^{n} C\__i $$

`$$\sum_{n=1}^{\infty} 2^{-n} = 1$$`

##### References:

[https://stackoverflow.com/questions/12128221/how-to-quickly-find-the-maximum-average-interval](https://stackoverflow.com/questions/12128221/how-to-quickly-find-the-maximum-average-interval)

https://activities.tjhsst.edu/sct/lectures/1112/binary102111.pdf

