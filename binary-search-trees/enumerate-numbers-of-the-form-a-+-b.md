#### Enumerate Numbers of the Form a + b$$\small \sqrt{2}$$

> Numbers of the form $$\small a + b\sqrt{q}$$, where $$\small a, b$$ are nonnegative integers, and $$\small q$$ is an integer which is not the square of another integer, have special properties, e.g., they are closed under addition and multiplication. Design an algorithm for efficiently computing the $$\small k$$ smallest numbers of the form $$\small a + b\sqrt{2}$$ for nonnegative integers $$\small a$$ and $$\small b$$.

##### Code:

```py
class Number:
    def __init__(self, a, b):
        self.a, self.b = a, b
        self.val = a + b*math.sqrt(2)

    def __lt__(self, other):
        return self.val < other.val

    def __eq__(self, other):
        return self.val == other.val


def generate_first_k_a_b_sqrt2(k):
    # Initial setup for 0 + 0 * sqrt(2)
    candidates = bintrees.RBTree([(Number(0, 0), None)])

    res = []

    while len(res) < k:
        next_smallest = candidates.pop_min()[0]
        res.append(next_smallest.val)
        candidates[Number(next_smallest.a, next_smallest.b + 1)] = None
        candidates[Number(next_smallest.a + 1, next_smallest.b)] = None
    return res
```



