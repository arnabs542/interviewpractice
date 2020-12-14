#### Snapshot Array - LC 1146

> Implement a SnapshotArray that supports the following interface:
>
> * `SnapshotArray(int length)` initializes an array-like data structure with the given length.  Initially, each element equals `0`.
> * `void set(index, val)` sets the element at the given index to be equal to `val`.
> * `int snap()` takes a snapshot of the array and returns the `snap_id`: the total number of times we called `snap()` minus `1`.
> * `int get(index, snap_id)` returns the value at the given index, at the time we took the snapshot with the given `snap_id`
>
> Example 1:
> ```
> Input: ["SnapshotArray","set","snap","set","get"]
> [[3],[0,5],[],[0,6],[0,0]]
> Output: [null,null,0,null,5]
> Explanation: 
> SnapshotArray snapshotArr = new SnapshotArray(3); // set the length to be 3
> snapshotArr.set(0,5);  // Set array[0] = 5
>snapshotArr.snap();  // Take a snapshot, return snap_id = 0
>snapshotArr.set(0,6);
>snapshotArr.get(0,0);  // Get the value of array[0] with snap_id = 0, return 5

##### Solution

The brute force solution would be to maintain an array, then for every snapshot, turn the array into a tuple and store it with the `snap_id` as the key. However, this is quite expensive memory and time wise to do, and we end up storing a bunch of information we may not need. For example, if we have a series of 10 update statements operating on the same index, it doesn't make sense to store all the other values constantly when they don't change. 

We don't actually even need to maintain an array to store the values. Since we are told all values are initally set to 0, and the changes aren't permanent until we make a call to `snap()`, all we have to do is for each index, store an ordered array of `(snap_id, value)`.

This allows us to preform `set()` and `snap()` in $\small \mathcal O(1)$ time and `get()` in $\small \mathcal O(n \log{n})$ time. 

```py
class SnapshotArray:

    def __init__(self, length: int):
        self.changes = collections.defaultdict(list)
        self.snap_id = 0
        

    def set(self, index: int, val: int) -> None:
        if not self.changes[index] or self.snap_id > self.changes[index][-1][0]:
            self.changes[index].append([self.snap_id, val])
        else:
            self.changes[index][-1][1] = val
            
    def snap(self) -> int:
        self.snap_id += 1
        return self.snap_id - 1
        

    def get(self, index: int, snap_id: int) -> int:
        # Index hasn't been changed or before first change
        if not self.changes[index] or snap_id < self.changes[index][0][0]:
            return 0
        change_history = self.changes[index]
        # Find where the snap_id is
        lo, hi = 0, len(change_history) - 1
        while lo <= hi:
            mid = (lo + hi) >> 1
            if change_history[mid][0] == snap_id:
                return change_history[mid][1]
            elif change_history[mid][0] > snap_id:
                hi = mid - 1
            else:
                lo = mid + 1
        
        return change_history[lo-1][1]
```