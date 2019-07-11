#### Compress a 2D Array

> Given a `2D` array of distinct integers with n numbers, "compress" the array such that the resulting array's numbers are in the range `[1, n]` and their relative order is kept.
> 
> Relative order means that if a number at position `(row1, col1)` is smaller than a number at position `(row2, col2)` in the original array, it should still have that relationship in the resulting array.
> 
> Example:
> ```
> Input:
> 7 6
> 4 9
> 
> Output:
> 3 2
> 1 4
>```
>Follow-up :
> Compress the array to make the numbers as small as possible as long as the relative order holds true for numbers in the same row and column.
>
> Example 1:
> ```
> Input:
> 7 6
> 4 9
>
> Output:
> 2 1
> 1 2
> ```
> Example2:
> ```
> Input:
> 25 74 54
> 12 56 83
>
> Output:
> 2 4 3
> 1 2 4
>```
>Example3:
> ```
> Input:
> 20 80 60 70
> 11 90 22 44
> 33 99 49 88
>
> Output:
> 2 7 5 6
> 1 8 2 3
> 3 9 4 7
> ```

#### Solution

The original problem is simple enough - simply put the elements in a heap along with their coordinates. Starting from `1`, continue popping from the heap until it's empty, updating the original value to the new compressed value based on the order it's popped. Time complexity is $\small \mathcal O(n \log{n})$.

Follow-up is a little trickier - there is no universal ordering like in the first problem. As we update each number, it affects every other number in the row and column. However, we can continue to use our heap approach as long as we keep a maximum of the compressed row and column. 

```py
import heapq

def compress_2d_followup(arr):
    heap = []
    for i in len(arr):
        for j in len(arr[0]):
            heapq.heapush(heap, (arr[i][j], i, j))
    
    col_max = [0] * len(arr[0])
    row_max = [0] * len(arr[1])

    while heap:
        _, i, j = heapq.heappop(heap)
        arr[i][j] = col_max[j] = row_max[i] = max(col_max[j], row_max[i]) + 1
```

There is some inefficiency in computing the maximum of each column and row in the above solution - we can update those as we populate the matrix. Running time is $\small \mathcal O(mn)$, space is $\small \mathcal O(m + n)$.