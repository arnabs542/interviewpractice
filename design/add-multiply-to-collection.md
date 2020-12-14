#### Add/Multiply to Collection

> Design an integer collection that supports `addToAll` and `multiplyToAll` opearations that respectively adds and multiplies a given number to all the elements in the collection at this point.
> ```cpp
> class AddToAllCollection {
>    public void append(int val) {
>    }
>
>    public int get(int index) {
>    }
>
>    public void addToAll(int val) {
>    }
> }
> ```
> All methods should work in O(1) time.
>
> Example:
> ```cpp
> AddToAllCollection col = AddToAllCollection(); // []  initialy collection is empty
> col.append(1); // [1]
> col.append(2); // [1, 2]
> col.append(3); // [1, 2, 3]
> col.get(2); // return 3
> col.addToAll(1); // [2, 3, 4]
> col.get(2); // return 4
> col.addToAll(5); // [7, 8, 9]
> col.get(2); // return 9
> col.append(4); // [7, 8, 9, 4]
> col.get(3); // return 4, because the newly appended number was not involved with the previous addToAll calls.
>```

##### Solution

Let's break the problem apart first and deal simply with `addToAll`. The function `append` appends a value to the end, and the function `get` refers to a specific index, so that suggests using a list as our backbone. This will indeed satisfy the $\small \mathcal O(1)$ time requirement. 

To implement the `addToAll` function in constant time, we use an additional variable to keep track of the overall delta. Every new element added will first subtract the delta to keep consistent with the rest of the array.

```py
class AddToAllCollections:
    def __init__(self):
        self.values = []
        self.addition = 0
    
    def append(self, val):
        self.values.append(val - self.addition)
    
    def get(self, idx):
        if idx >= len(self.values):
            raise IndexError('list index out of range')
        return self.values[idx] + self.addition
    
    def add_to_all(self, val):
        self.addition += val
```

Now let's look at `multiplyToAll`. We could try using a similar approach to implementing `addToAll`, but that means we have to store floats instead of ints, taking up a lot more memory. Furthermore, if we get a `0` as a multiple, we will run into issues trying to divide by it. 

Instead, we store another list to keep track of the factor the number. For example:

```py
vals = [1], factor = [1]
multiple = 1

append(5)
vals = [1,5], factor = [1,1]
multiple = 1

multiply_to_all(3)
vals = [1,5], factor = [1,1]
multiple = 3

append(10)
vals = [1,5,10], factor = [1,1,3]
multiple = 3
```

The variable `multiple` keeps track of the total product from all the `multiply_to_all` calls. The `factor` array keeps track of the factor of the number at that index relative to the multiple. So for the first two numbers, both of them will be returned by a factor of `3/1 = 3`, while `10` will be returned by a factor of `3/3 = 1`.

To deal with multiplying by `0`, we keep an index marking the last time we encountered a `0`. Everything within that bound returns a `0`.

```py
class MultiplyToAllCollections:
    def __init__(self):
        self.values = []
        self.factors = []
        self.multiple = 1
        self.set_to_zero = -1
    
    def append(self, val):
        self.values.append(val)
        self.factors.append(multiple)
    
    def get(self, idx):
        if idx >= len(self.values):
            raise IndexError('list index out of range')
        elif idx <= self.set_to_zero:
            return 0
        else:
            return self.values[idx] * self.multiple / self.factors[idx]
    
    def multiply_to_all(self, val):
        if val == 0:
            self.multiple = 1
            self.set_to_zero = len(self.values) - 1
        else:
            self.multiple *= val
```

Now let's put it all together:
```py
class AddAndMultiplyToAllCollections:
    def __init__(self):
        self.values = []
        self.factors = []
        self.addition = 0
        self.multiple = 1
        self.set_to_zero = -1
    
    def append(self, val):
        self.values.append(val - self.addition)
        self.factors.append(multiple)
    
    def get(self, idx):
        if idx >= len(self.values):
            raise IndexError('list index out of range')
        elif idx <= self.set_to_zero:
            return self.addition
        else:
            return (self.values[idx]) * self.multiple / self.factors[idx] + self.addition

    def add_to_all(self, val):
        self.addition += val
    
    def multiply_to_all(self, val):
        if val == 0:
            self.multiple = 1
            self.additions = 0
            self.set_to_zero = len(self.values) - 1
        else:
            self.multiple *= val
            self.addition *= val
```

The only additional adjustment we must make is to multiply `self.addition` for all previous values too.