# Binary Search

A search problem is a good candidate for binary search if large chunks of the search space can be discarded each time. For example, when searching for a number in a sorted array, if the number we picked was too small, then we can discard and everything to the left of it. If the number is too big, we can discard it and everything to the right of it. We may need to sort the array before we can apply binary search, but the initial time penalty taken to sort can dramatically speed up future work.

A binary search algorithm usually consists of 3 components:

1. **Pre-processing**: Convert the array to a sorted form 
2. **Binary search**: Divide the search space into half iteratively or recursively
3. **Post-processing**: Determine viable candidates in the remaining search space

Not all steps will be used all the time.

Per the LeetCode explore topic, there are three main templates used in binary search problems.

#### Template 1:

```py
def binarySearch(nums, target):
    if len(nums) == 0:
        return -1

    left, right = 0, len(nums) - 1
    while left <= right:
        mid = (left + right) // 2
        if nums[mid] == target:
            return mid
        elif nums[mid] < target:
            left = mid + 1
        else:
            right = mid - 1

    # End Condition: left > right
    return -1
```

Template 1 is usually the most common template presented. It's useful when the element or condition can be checked via _accessing a single index_. 

**Key Attributes:**

* Initial bounds are inclusive
* Search Condition can be determined without comparing to the element's neighbors \(or use specific elements around it\)
* No post-processing required because the termination condition requires `left > right`, which means there are no candidates left in the search space when the loop terminates. 

**Distinguishing Syntax:**

* Initial Condition: `left = 0, right = length-1`
* Termination: `left > right`
* Searching Left: `right = mid-1`
* Searching Right:`left = mid+1`

#### Template 2:

```py
def binarySearch(nums, target):
    if len(nums) == 0:
        return -1

    left, right = 0, len(nums)
    while left < right:
        mid = (left + right) // 2
        if nums[mid] == target:
            return mid
        elif nums[mid] < target:
            left = mid + 1
        else:
            right = mid

    # Post-processing:
    # End Condition: left == right
    if left != len(nums) and nums[left] == target:
        return left
```



