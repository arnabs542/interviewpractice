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

Template 2 differs in that it is used to search for an element or condition which requires _accessing the current index and its immediate right neighbor's index_ in the array.

**Key Attributes:**

* Search Condition needs to access element's immediate right neighbor
* Use element's right neighbor to determine if condition is met and decide whether to go left or right
* Gurantees Search Space is at least 2 in size at each step
* Post-processing required. Loop/Recursion ends when you have 1 element left. Need to assess if the remaining element meets the condition.

**Distinguishing Syntax:**

* Initial Condition: `left = 0, right = length`
* Termination: `left == right`
* Searching Left: `right = mid`
* Searching Right: `left = mid+1`



