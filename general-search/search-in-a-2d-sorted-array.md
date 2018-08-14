#### Search a 2D Matrix

> Write an efficient algorithm that searches for a value in an _m_ x _n_ matrix. This matrix has the following properties:
>
> * Integers in each row are sorted from left to right.
> * The first integer of each row is greater than the last integer of the previous row.
>
> **Example 1:**
>
> ```
> Input:
>
> matrix = [
>   [1,   3,  5,  7],
>   [10, 11, 16, 20],
>   [23, 30, 34, 50]
> ]
> target = 3
>
> Output: true
> ```
>
> **Example 2:**
>
> ```
> Input:
>
> matrix = [
>   [1,   3,  5,  7],
>   [10, 11, 16, 20],
>   [23, 30, 34, 50]
> ]
> target = 13
>
> Output: false
> ```



##### Binary Search Code:

```py
def searchMatrix(self, matrix, target):

    if not matrix or not matrix[0] or target > matrix[-1][-1] or target < matrix[0][0]:
        return False

    row = None

    # Find candidate row
    lo, hi = 0, len(matrix) - 1
    while lo <= hi:
        mid = (lo + hi) >> 1
        # Found candidate row
        if matrix[mid][0] <= target and matrix[mid][-1] >= target:
            row = matrix[mid]
            break
        # Row is too small
        elif matrix[mid][-1] < target:
            lo = mid + 1
        # Row is too big
        elif matrix[mid][0] > target:
            hi = mid - 1

    if not row:
        return False

    # Find num in row
    lo, hi = 0, len(row) - 1
    while lo <= hi:
        mid = (lo + hi) >> 1
        if row[mid] == target:
            return True
        elif row[mid] < target:
            lo = mid + 1
        else:
            hi = mid - 1

    return False
```

##### Explanation:

The fact that both row and columns are ordered provides us the necessary conditions to directly apply binary search. First we search all rows to see if there is a candidate row. Upon finding the candidate row, if it exists, we then perform binary search in the row itself to search for the element. Overall runtime is $$\small \mathcal O(\log(r) + \log(c))$$, where $$\small r, c$$ represent the number of rows and columns in the matrix, respectively. 

##### Linear Search Code:

```py
def searchMatrix(self, matrix, target):

    if not matrix or not matrix[0]:
        return False

    row, col = 0, len(matrix[0]) - 1
    while row < len(matrix) and col >= 0:
        if matrix[row][col] == target:
            return True
        elif matrix[row][col] < target:  # Disregard row
            row += 1
        else:   # Disregard col
            col -= 1
    return False
```

##### Explanation:

We can also use a simple linear search to find the element in $$\small \mathcal O(r + c)$$ time. The trick is to compare extremes. Suppose we compare $$\small A[0][c-1]$$ with our target. There are two possibilities, assuming that cell does not equal to the target:

* $$\small A[0][c-1] < x$$, in which case $$\small x$$ is greater than all elements in Row 0

* $$\small A[0][c-1] > x$$, in which case $$\small x$$ is smaller than all elements in Column $$\small c - 1$$.

* 
* 
* 


