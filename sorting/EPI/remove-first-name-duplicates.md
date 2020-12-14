#### Remove First-Name Duplicates

> Design an efficient algorithm for removing all first-name duplicates from an array. For example, if the input is &lt;\(Ian,Botham\), \(David,Gower\),\(Ian,Bell\),\(Ian,Chappell\)&gt;, one result could be &lt;\(Ian,Bell\),\(David,Gower\)&gt;;&lt;\(David,Gower\),\(Ian,Botham\)&gt; would also be acceptable.

##### Code \(Standard\):

```py
class Name:
    def __init__(self, first_name, last_name):
        self.first_name, self.last_name = first_name, last_name

    def __lt__(self, other):
        return (self.first_name < other.first_name
                if self.first_name != other.first_name else
                self.last_name < other.last_name)


def eliminate_duplicate(A):
    A.sort()
    write_idx = 1
    for name in A[1:]:
        if name.first_name != A[write_idx - 1].first_name:
            A[write_idx] = name
            write_idx += 1
    del A[write_idx:]
```

##### Explanation:

We could use a hash table to keep track of all first names seen so far and then iterate over the array, adding only new names. This takes $\small \mathcal O(n)$ space and time. To save a bit of space at the expense of time, we can sort the array, which groups all names with the same first name. This takes $\small \mathcal O(n \log{n})$ time, but requires no additional space.

The problem itself is rather mundane, but it's important to note that for objects to be comparable, we need to define a comparison function so that Python knows what property to use to compare. In the `Name` object above, we define the less than comparator function so that we know what sorting an array of names means.

##### Code \(Pythonic\): 

```py
def eliminate_duplicate_pythonic(A):
    A.sort()
    write_idx = 0
    for cand, _ in itertools.groupby(A):
        A[write_idx] = cand
        write_idx += 1
    del A[write_idx:]
```



