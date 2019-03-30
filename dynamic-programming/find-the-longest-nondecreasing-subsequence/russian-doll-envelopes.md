#### Russian Doll Envelopes

> You have a number of envelopes with widths and heights given as a pair of integers `(w, h)`. One envelope can fit into another if and only if both the width and height of one envelope is greater than the width and height of the other envelope.
>
> What is the maximum number of envelopes can you Russian doll? \(put one inside other\)
>
> **Note:**  
>  Rotation is not allowed.
>
> **Example:**
>
> ```
> Input: [[5,4],[6,4],[6,7],[2,3]]
> Output: 3 
>
> Explanation: 
> The maximum number of envelopes you can Russian doll is 3 ([2,3] => [5,4] => [6,7]).
> ```

##### Dynamic Programming:

We could approach this same way we approach the LIS problem:

```py
def maxEnvelopes(envelopes: "List[List[int]]") -> int:
    n = len(envelopes)
    if n == 0:
        return 0
    
    envelopes.sort(key=lambda x: (x[0]))        
    lis = [1] * n
    
    for i in range(1, n):
        for j in range(i):
            if envelopes[i][0] > envelopes[j][0] and envelopes[i][1] > envelopes[j][1]:
                lis[i] = max(lis[i], lis[j] + 1)
    
    return max(lis)
```

Since, we're not given any restrictions on the ordering, we can sort the array. We pick one feature to sort \(weight in the above code\), and then iterate through the dolls, checking for the longest subsequence that can be formed among all previous dolls that can be fit into the current one. 

Running time is $$\small \mathcal O(n^{2})$$, and space is $$\small \mathcal O(n)$$.

For large inputs, we obviously want use the $$\small \mathcal O(n\log{n})$$ approach. The difficulty in applying that directly to this problem is that we have two factors to compare, instead of a single number. In order to use the binary search method, we need to some how take care of a factor independently:

```py
def bin_search(lis: "List[List[int]]", doll: "List[int]"):
    lo, hi = 0, len(lis) - 1
    while lo < hi:
        mid = (lo + hi) >> 1
        if lis[mid][1] < doll[1]:
            lo = mid + 1
        else:
            hi = mid
    return lo

def maxEnvelopes(envelopes: List[List[int]]) -> int:
    n = len(envelopes)
    if n == 0:
        return 0
    
    envelopes.sort(key=lambda x: (x[0], -x[1]))        
    lis = []
    
    for doll in envelopes:
        if not lis or doll[0] > lis[-1][0] and doll[1] > lis[-1][1]:
            lis.append(doll)
        else:
            idx = bin_search(lis, doll)
            lis[idx] = doll                
    
    return len(lis)
```



