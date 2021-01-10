## Find Minimum Time to Finish All Jobs

>You are given an integer array jobs, where jobs[i] is the amount of time it takes to complete the ith job.
>
>There are k workers that you can assign jobs to. Each job should be assigned to exactly one worker. The working time of a worker is the sum of the time it takes to complete all jobs assigned to them. Your goal is to devise an optimal assignment such that the maximum working time of any worker is minimized.
>
>Return the minimum possible maximum working time of any assignment.
>
> 
>
>Example 1:
>```
>Input: jobs = [3,2,3], k = 3
>Output: 3
>Explanation: By assigning each person one job, the maximum time is 3.
>```
>Example 2:
>```
>Input: jobs = [1,2,4,7,8], k = 2
>Output: 11
>Explanation: Assign the jobs the following way:
>Worker 1: 1, 2, 8 (working time = 1 + 2 + 8 = 11)
>Worker 2: 4, 7 (working time = 4 + 7 = 11)
>The maximum working time is 11.
>```
> 
>
>Constraints:
>
>    - 1 <= k <= jobs.length <= 12
>    - 1 <= jobs[i] <= 107
>

This problem actually is a pretty standard backtracking problem. The only trick here is how to prune the search tree aggressively. 

The first condition we can check before continuing our recursion is to see whether or not we can get a strictly better result. If by assigning a job to a particular worker we are guaranteed to get a minimum completion time worse or equal to what we already found, then break the recursion.

The second condition is to check if we've already made a similar assignment. For example, suppose after processing `i` jobs, we have an assignment array like: `[10, 5, 5, 5, 5]`. It doesn't make sense to try assigning `jobs[i]` to every single worker currectly with a completion time of `5`, because it will not affect the final answer. So if we already tried assining the current job to a worker with some hours `h`, then we can skip all other assignments to any worker also with some hours `h`. Note that this pruning needs to applied per job instead of globally.


```py
def minimumTimeRequired(jobs: List[int], k: int) -> int:
    
    workers = [0] * k
    self.best = float('inf')
    seen = set()
    
    
    def helper(idx):
        if idx >= len(jobs):
            self.best = min(self.best, max(workers))
            return
        
        seen = set()
        
        for i in range(k):
            # Prune obviously worse assignments
            if workers[i] + jobs[idx] >= self.best:
                continue
            # Check if we've already tried a similar assignment
            if workers[i] in seen:
                continue
            
            seen.add(workers[i])
            workers[i] += jobs[idx]
            helper(idx+1)
            workers[i] -= jobs[idx]
        
    helper(0)
    return self.best
```

Running time is bounded by $O(n^{k})$. Space complexity is $O(max(n, k))$ - we are either bounded by the number of jobs or number of workers. 