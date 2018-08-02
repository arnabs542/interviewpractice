# Search A Sorted Array for First Occurrence of k

Write a method that takes a sorted array and a key and returns the index of the _first_ occurrence of that key in the array. Return -1 if the key does not appear in the array.



The binary search application is fairly straight forward. What is important is pay attention to the boundary setting and the termination conditions. 

```py
def search_first_of_k(A, k):
    idx = None
    left, right = 0, len(A)
    while left < right:
        mid = left + (right - left) // 2
        if A[mid] == k:
            idx = mid
            right = mid
        elif A[mid] > k:
            right = mid
        else:
            left = mid + 1
    return idx if idx is not None else -1

```

The code above has an inclusive lower bound and an exclusive upper bound. In this situation, our terminating condition can only be left &lt; right, otherwise we may go out of bounds when shifting our lower bound. In the case of A\[mid\] &gt; target, we set right = mid, because the upper bound is exclusive, and we already verified that mid is not the solution. If we set right = mid - 1, we don't know if mid - 1 is the solution, and we could end up skipping over it. 

We can also use an inclusive upper bound, which means we terminate with left &lt;= right, otherwise we'll end up skipping over a final element. 

```py
def search_first_of_k(A, k):
    idx = None
    left, right = 0, len(A) - 1
    while left <= right:
        mid = left + (right - left) // 2
        if A[mid] == k:
            idx = mid
            right = mid - 1
        elif A[mid] > k:
            right = mid - 1
        else:
            left = mid + 1
    return idx if idx is not None else -1
```

Suppose array is \[4, 5, 6\] ans we want to search for element 6. Length of array here is 3.



Initialization: We set low = 0. We can either set high = 2 or high = 3. \(i.e. Length -1 or Length\)



Running loop with high initialized with Length - 1

low = 0, high = 2



In first iteration



    low = 0, high = 2 and middle = 1.    array\[middle\] is 5, so low = middle + 1 i.e. 2



On second iteration with if low &lt; high: loop would terminate without searching for element at location 2, so it should be if low &lt;= high:



In second iteration with if low &lt;= high: 



    low = 2, high = 2, middle = 2, array\[middle\] == x, so we return 2.

Running loop with high initialized with Length

low = 0, high = 3



In first iteration    low = 0, high = 3 and middle = 1.    array\[middle\] is 5, so low = middle + 1 i.e. 2



In second iteration with if low &lt;= high:     low = 2, high = 3, middle = 2. array\[middle\] == x, we return 2.



Note that condition must not be if low &lt;= high: because if x is not present in array it will cause loop to run low = 3 and high = 3 in second iteration and that will cause index out of bounds when loop runs for 3rd time. 



