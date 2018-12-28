#### Partitioning and Sorting an Array with many Repeated Entries

> Suppose you need to reorder the elements of a very large array so that equal elements appear together. For example, if the array is $$\small <b,a,c,b,d,a,b,d>$$ then $$\small <a,a,b,b,b,c,d,d>$$ is an acceptable reordering, as is $$\small <d,d,c,a,a,b,b,b>$$.
>
> If the entries are integers, this reordering can be achieved by sorting the array. If the number of distinct integers is very small relative to the size of the array, an efficient approach to sorting the array is to count the number of occurrences of each distinct integer and write the appropriate number of each integer, in sorted order, to the array. When array entries are objects, with multiple fields, only one of which is to be used as a key, the problem is harder to solve.
>
> You are given an array of student objects. Each student has an integer-valued age field that is to be treated as a key. Rearrange the elements of the array so that students of equal age appear together. The order in which different ages appear is not important. How would your solution change if ages have to appear in sorted order?

##### Default Sort:

```py
Person = collections.namedtuple('Person', ('age', 'name'))

def group_by_age(people):
    people.sort(key=lambda p: p.age)
    return
```

Since we're only sorting all objects based on a single field, which also happens to an integer, we can simply use the native sort function specifying the key as the age of the person. Running time is $$\small \mathcal O(n \log{n})$$.

##### Counting:

```py
Person = collections.namedtuple('Person', ('age', 'name'))


def group_by_age(people):
    age_to_count = collections.Counter((person.age for person in people))   # Counts how many times each age appears
    age_to_offset, offset = {}, 0   # Sets the beginning indices of each group

    for age, count in age_to_count.items():
        age_to_offset[age] = offset
        offset += count

    while age_to_count:
        from_age = next(iter(age_to_count))
        from_idx = age_to_offset[from_age]
        to_age = people[from_idx].age
        to_idx = age_to_offset[to_age]
        people[from_idx], people[to_idx] = people[to_idx], people[from_idx]
        # Check if we are done with an age group
        age_to_count[to_age] -= 1
        if age_to_count[to_age]:
            age_to_offset[to_age] = to_idx + 1
        else:
            del age_to_count[to_age]
```

If we do not need to have the groups of ages be in sorted order, then a naïve sort does more than what is required. We can actually perform the grouping in $$\small \mathcal O(n)$$ time by counting the frequencies of each age.

For example, suppose there were two age groups: 14, and 15. If there were 3 students aged 14, and 5 students aged 15, then we block off the first 4 elements in the array for the students aged 14, resulting in the offset for 15 to be set to 4 at first:

```py
age_to_offset[14] = 0, age_to_count[14] = 4
age_to_offset[15] = 4, age_to_count[15] = 5
```

We then iterate through our `age_to_count`, and each time we move one element to its correct position in the array. So if we initially start with 14, we will have:

```py
from_age = 14
from_idx = age_to_offset[14] = 0
to_age = people[0].age
to_idx = age_to_offset[to_age]
```

We use the `age_to_count` hash table to keep track of which age groups haven't been completely processed yet. Each iteration, we ask the hash to give us one number \(`from_age`\). We then see where that number needs to go by looking its position in the `age_to_offset` hash table \(`from_idx`\). Next, we find the number age at `people[from_idx]`, and we find its correct position `to_idx`. We move the number originally at `from_idx` to its correct position.

The initially confusing part of this algorithm is that we don't actually move the element we get from the hashtable implicitly. So for example, if the hashtable gives a 14, we see where the next 14 should be, and if the number at that index is a 14, we "move" it by reassigning it to its current location. If the number in that place is a 15, we actually end up working on the age 15 subgroup by moving the 15 to its correct position, despite the hash table giving a 14. It's entirely possible for us to be "stuck" on a number longer than the amount of times it occurs in the array, because each time we could end up moving a number from another group. However, each iteration results in one more number being placed in its correct position, meaning that the running time is still bounded by $$\small \mathcal O(n)$$.

