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



