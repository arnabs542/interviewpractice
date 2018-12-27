#### Maximum Width Ramp

> Given an array `A` of integers, a _ramp_ is a tuple `(i, j)` for which `i < j` and `A[i] <= A[j]`.  The width of such a ramp is `j - i`.
>
> Find the maximum width of a ramp in `A`.  If one doesn't exist, return 0.
>
> **Example 1:**
>
> ```
> Input: [6,0,8,2,1,5]
> Output: 4
>
> Explanation: 
> The maximum width ramp is achieved at (i, j) = (1, 5): A[1] = 0 and A[5] = 5.
> ```
>
> **Example 2:**
>
> ```
> Input: [9,8,1,0,1,9,4,0,4,1]
> Output: 7
>
> Explanation: 
> The maximum width ramp is achieved at (i, j) = (2, 9): A[2] = 1 and A[9] = 1.
> ```

##### Sorting:

```py
def maxWidthRamp(A):

    min_ind, max_ramp, inds = float('inf'), 0, collections.defaultdict(list)

    for i,e in enumerate(A):
        inds[e].append(i)

    for i,e in enumerate(sorted(A)):
        max_ramp = max(max_ramp, inds[e][-1] - min_ind)
        min_ind = min(min_ind, inds[e][0])

    return max_ramp
```

Another way of rephrasing this problem would be for every number, find the earliest occurrence of a number less than or equal to it. Of course, we could just use a nested for loop, but that quickly times out.

The idea above can be summarized as such:

* Initial max\_ramp = 0
* Initial min index \(min\_ind\) = float\("inf"\)

* Record every index of every number in "index" collection because there can be duplicate numbers.

* Sort A

* For every number in sorted A, get difference between last index of current number \(index\[num\]\[-1\]\) and minimum index of previous numbers \(ind\).

The runtime is dominated by the sort; $$\small \mathcal O(n \log{n})$$. Space is $$\small \mathcal O(n)$$.

##### Sorting \(No Space\):

```py
def maxWidthRamp(A):

    min_idx, max_ramp = float('inf'), 0

    for i in sorted(range(len(A)), key=A.__getitem__):
        max_ramp = max(max_ramp, i - min_idx)
        min_idx = min(min_idx, i)

    return max_ramp
```

The idea is the same as the algorithm above. However, this time we generate a new array that contains the indices of the sorted original array. For example, suppose the original array $$\small A$$ was $$\small [6,0,8,2,1,5]$$. After sorting, we get the array $$\small [1, 4, 3, 5, 0, 2]$$.  This is because $$\small 0$$ is the smallest element in $$\small A$$, and it's at position $$\small 1$$, $$\small 1$$ is the second smallest element, and it's at position $$\small 4$$, and so on and so forth.

* Initial max\_ramp = 0
* Initial min\_idx = float\("inf"\)
* Sort A by values but keep indicies
* For every number in the sorted array, check to see if the number at the original array could give us a larger max\_ramp compared to the previous min\_ind index, and then update min\_ind as needed.

Runtime is still dominated by sort, but space is now constant.

##### Stack:

