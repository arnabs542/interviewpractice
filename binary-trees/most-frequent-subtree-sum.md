#### Most Frequent Subtree Sum

> Given the root of a tree, you are asked to find the most frequent subtree sum. The subtree sum of a node is defined as the sum of all the node values formed by the subtree rooted at that node \(including the node itself\). So what is the most frequent subtree sum value? If there is a tie, return all the values with the highest frequency in any order.
>
> **Examples 1**  
> Input:
>
> ```
>   5
>  /  \
> 2   -3
> ```
>
> return \[2, -3, 4\], since all the values happen only once, return all of them in any order.
>
> **Examples 2**  
> Input:
>
> ```
>   5
>  /  \
> 2   -5
> ```
>
> return \[2\], since 2 happens twice, however -5 only occur once.

##### DFS \(Postorder\):

```py
def findFrequentTreeSum(root):
    
    if not root:
        return []
    
    def helper(root, sums):
        if not root:
            return 0
        cur_sum = helper(root.left, sums) + helper(root.right, sums) + root.val
        sums[cur_sum] += 1            
        return cur_sum
    
    sums = collections.defaultdict(int) 
    helper(root, sums)
    max_freq = max(sums.values())
    return [k for k in sums if sums[k] == max_freq]
```

We're told that the subtree sum of a node is defined as the sum of **all** of the node values formed by the subtree root at that node \(including the node itself\). This means we can just perform a postorder traversal to calculate each subtree sum bottom up. 

Running time is $$\small \mathcal O(n)$$. 

**Edge Cases**:

1. Watch out for null inputs
2. At first I was trying to build my result while traversing - updating maximum frequency, building result list, etc. However, it would be better to just do it in one go at the end of the traversal. 



