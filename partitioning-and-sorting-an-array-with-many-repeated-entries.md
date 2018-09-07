#### Partitioning and Sorting an Array with many Repeated Entries

> Suppose you need to reorder the elements of a very large array so that equal elements appear together. For example, if the array is $$\small <b,a,c,b,d,a,b,d>$$ then $$\small <a,a,b,b,b,c,d,d>$$ is an acceptable reordering, as is $$\small <d,d,c,a,a,b,b,b>$$.
>
> If the entries are integers, this reordering can be achieved by sorting the array. If the number of distinct integers is very small relative to the size of the array, an efficient approach to sorting the array is to count the number of occurrences of each distinct integer and write the appropriate number of each integer, in sorted order, to the array. When array entries are objects, with multiple fields, only one of which is to be used as a key, the problem is harder to solve.
>
> You are given an array of student objects. Each student has an integer-valued age field that is to be treated as a key. Rearrange the elements of the array so that students of equal age appear together. The order in which different ages appear is not important. How would your solution change if ages have to appear in sorted order?

##### Code \(Default Sort\):

```
Person = collections.namedtuple('Person', ('age', 'name'))

def group_by_age(people):
    people.sort(key=lambda p: p.age)
    return
```

##### Explanation:

Since we're only sorting all objects based on a single field, which also happens to an integer, we can simply use the native sort function specifying the key as the age of the person. Running time is $$\small \mathcal O(n \log{n})$$.



