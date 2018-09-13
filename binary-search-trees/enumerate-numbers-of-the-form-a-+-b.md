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

##### Explanation:

Since $$\small \sqrt{2}$$ is an irrational number, our numbers are essentially composed of two distinct components: an integer component $$\small a$$, and some integer multiple of $$\small \sqrt{2}$$. Each consecutive number must be one more component than an earlier number. For example, the first number is $$\small 0 + 0\sqrt{2}$$. The next smallest number must either be $$\small 1 + 0\sqrt{2}$$ or $$\small 0 + 1\sqrt{2}$$. This allows us to generate the next candidates by taking the current smallest element, and adding either a $$\small 1$$ or a $$\small \sqrt{2}$$ to it. A BST allows us to quickly extract the smallest number, and it will also automatically place the new entries in their correct positions. Thus, we continue to extract the smallest elements, increase the components, then insert them back into the tree until we have our $$\small k$$ elements. 

Running time is $$\small \mathcal O(k \log{k})$$, since we go through $$\small k$$ iterations, and in each iteration we do 1 deletion and 2 insertions. Space is bounded by $$\small \mathcal O(k)$$, since we insert at most $$\small 2k$$ elements. 

