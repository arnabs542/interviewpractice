#### Insert Delete GetRandom O(1) - Duplicates allowed
> Design a data structure that supports all following operations in average O(1) time.
> Note: Duplicate elements are allowed.
> 
    > 1. insert(val): Inserts an item val to the collection.
    > 2. remove(val): Removes an item val from the collection if present.
    > 3. getRandom: Returns a random element from current collection of elements. The probability of each element being returned is linearly related to the number of same value the collection contains.
> 
> Example:
>
>```
>// Init an empty collection.
>RandomizedCollection collection = new RandomizedCollection();
>
> // Inserts 1 to the collection. Returns true as the collection did not >contain 1.
>collection.insert(1);
>
> // Inserts another 1 to the collection. Returns false as the >collection contained 1. Collection now contains [1,1].
>collection.insert(1);
>
> // Inserts 2 to the collection, returns true. Collection now contains >[1,1,2].
>collection.insert(2);
>
> // getRandom should return 1 with the probability 2/3, and returns 2 >with the probability 1/3.
>collection.getRandom();
>
>// Removes 1 from the collection, returns true. Collection now >contains [1,2].
>collection.remove(1);
>
>// getRandom should return 1 and 2 both equally likely.
>collection.getRandom();
>```

##### Array + Hashmap:

To insert an element in $\small O(1)$ time, we can either use an array or a linked list. But since we need to generate a random in $\small O(1)$ time, it makes sense to use an array since we can randomly generate an index in constant time. 

The difficulty in this problem lies with remove. Normally, to remove an element from a list takes linear time - we scan the list to find the element and remove it. However, if we knew all the indices of the value we are looking for, we can do this in constant time. Take the value and swap it with the very last element in the array, then pop the array. Be careful of the ordering of the operations.

```py
class RandomizedCollection:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.vals = []
        self.indices = collections.defaultdict(set)

    def insert(self, val: int) -> bool:
        """
        Inserts a value to the collection. Returns true if the collection did not already contain the specified element.
        """
        self.indices[val].add(len(self.vals))
        self.vals.append(val)
        return len(self.indices[val]) == 1
        

    def remove(self, val: int) -> bool:
        """
        Removes a value from the collection. Returns true if the collection contained the specified element.
        """       
        if not self.indices[val]:
            return False
        
        to_remove, last_val = self.indices[val].pop(), self.vals[-1]

        self.indices[last_val].add(to_remove)
        self.indices[last_val].discard(len(self.vals)-1)
        self.vals[to_remove] = last_val
        self.vals.pop()
        return True
        
    def getRandom(self) -> int:
        """
        Get a random element from the collection.
        """
        return choice(self.vals)
```