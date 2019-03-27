#### Number of Longest Increasing Subsequence

> Given an unsorted array of integers, find the number of longest increasing subsequence.
>
> **Example 1:**
>
> ```
> Input: [1,3,5,4,7]
>
> Output: 2
>
> Explanation: The two longest increasing subsequence are [1, 3, 4, 7] and [1, 3, 5, 7].
> ```
>
> **Example 2:**
>
> ```
> Input: [2,2,2,2,2]
>
> Output: 5
>
> Explanation:
> The length of longest continuous increasing subsequence is 1, and there are 5 subsequences' length is 1, so output 5.
> ```

##### Dynamic Programming

```py
def findNumberOfLIS(self, nums: List[int]) -> int:
    if not nums:
        return 0
    
    longest_sequence_ending_at_num = [1] * len(nums)
    count_of_longest_sequence_ending_at_num = [1] * len(nums)
    
    for i in range(1, len(nums)):
        for j in range(i):
            if nums[i] > nums[j]:
                if longest_sequence_ending_at_num[j] >= longest_sequence_ending_at_num[i]:
                    longest_sequence_ending_at_num[i] = 1 + longest_sequence_ending_at_num[j]
                    count_of_longest_sequence_ending_at_num[i] = count_of_longest_sequence_ending_at_num[j]
                elif longest_sequence_ending_at_num[j] + 1 == longest_sequence_ending_at_num[i]:
                    count_of_longest_sequence_ending_at_num[i] += count_of_longest_sequence_ending_at_num[j]
    
    longest_sequence = max(longest_sequence_ending_at_num)
    return sum(c for i,c in enumerate(count_of_longest_sequence_ending_at_num) if 
                longest_sequence_ending_at_num[i] == longest_sequence)
```

The solution is a slight modification of the parent problem. In additional to keeping track of the longest sequence ending at an element, we also keep track of the number of longest sequences ending at the element. 

We use the loop method to calculate the longest sequence ending at an element. If we find an element `nums[j]` smaller than `nums[i]` that also has a longest sequence greater than or equal to the current longest sequence at `nums[i]`, then we update `longest_sequence_ending_at_num[i]` and we also set `count_of_longest_sequence_ending_at_num[i] = count_of_longest_sequence_ending_at_num[j]`, since we can add `nums[i]` to any of the sequences that end with `nums[j]`. Otherwise, if we find an element `nums[j]` that is smaller than `nums[i]` and whose longest sequence\(s\) is 1 element less than the current longest sequence\(s\) ending with `nums[i]`, we add the count from `count_of_longest_sequence_ending_at_num[j]` to `count_of_longest_sequence_ending_at_num[i]`, since we can append `nums[i]` to the end of any of the sequences ending with `nums[j]` to get a sequence equal in length to its current longest sequence.

