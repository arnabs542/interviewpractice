#### Reducing Dishes

> A chef has collected data on the satisfaction level of his n dishes. Chef can cook any dish in 1 unit of time.
> 
> Like-time coefficient of a dish is defined as the time taken to cook that dish including previous dishes multiplied by its satisfaction level  i.e.  ```time[i]* satisfaction[i]```
> 
> Return the maximum sum of Like-time coefficient that the chef can obtain after dishes p reparation.
> 
> Dishes can be prepared in any order and the chef can discard some dishes to get this maximum value.
> 
>  
> 
> **Example 1:**
>```
> Input: satisfaction = [-1,-8,0,5,-9]
> Output: 14
>Explanation: After Removing the second and last dish, the maximum total Like-time coefficient will be equal to (-1*1 + 0*2 + 5*3 = 14). Each dish is prepared in one unit of time.
>``` 
> 
> **Example 2:**
> ```
> Input: satisfaction = [4,3,2]
> Output: 20
> Explanation: Dishes can be prepared in any order, (2*1 + 3*2 + 4*3 = 20)
> ```
> 
> **Example 3:**
> ```
> Input: satisfaction = [-1,-4,-5]
> Output: 0
> Explanation: People don't like the dishes. No dish is prepared.
> ```
> 
> **Example 4:**
> ```
> Input: satisfaction = [-2,5,-1,0,3,-3]
> Output: 35
> ```

 
##### Sorting:

If we simply brute force all subsets and their permutations, time complexity is ridiculously large. Instead, think about invariants. 

The problem isn't as simple as just keeping the positive value dishes. We can use negative value dishes as buffers to increase the gain from positive value dishes by making them later. Look at Example 1. 

Obviously, we want to make dishes with higher satisfaction scores later to maximize our total score. We reverse sort the dishes, because we definitely want to keep all positive score dishes, and we want to make higher-valued dishes later. If we drop any dishes, it'll the negative value ones. 

We iterate through the list, and see if adding that dish will help improve our overall score. If ```dish[i] > dish[j]``` and both dishes are negative and ```i < j```, we would never keep ```dish[j]``` and drop ```dish[i]```. The whole purpose of keeping negative dishes is to add buffers until we cook the positive dishes, so we would want to add only small negative values. 

```py
def maxSatisfaction(satisfaction: List[int]) -> int:
    satisfaction.sort(reverse=True)
    if satisfaction[0] <= 0:
        return 0
    
    max_score = cum_sum = cur_score = 0
    
    for i in range(len(satisfaction)):
        cur_score = cum_sum + cur_score + satisfaction[i]
        cum_sum += satisfaction[i]
        max_score = max(max_score, cur_score)
    
    return max_score
```

Overall time complexity is $\small O(n \log{n})$.


