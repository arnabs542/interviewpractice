#### Enumerate Numbers of the Form a + b$$\small \sqrt{2}$$

> Numbers of the form $$\small a + b\sqrt{q}$$, where $$\small a, b$$ are nonnegative integers, and $$\small q$$ is an integer which is not the square of another integer, have special properties, e.g., they are closed under addition and multiplication. Design an algorithm for efficiently computing the $$\small k$$ smallest numbers of the form $$\small a + b\sqrt{2}$$ for nonnegative integers $$\small a$$ and $$\small b$$.

##### Code \(BST\):

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

##### Code \(Array\):

```
def generate_first_k_a_b_sqrt2(k):
    cand = [Number(0, 0)]
    i = j = 0
    for _ in range(1, k):
        cand_i_plus_1 = Number(cand[i].a + 1, cand[i].b)
        cand_j_plus_sqrt2 = Number(cand[j].a, cand[j].b + 1)
        cand.append(min(cand_i_plus_1, cand_j_plus_sqrt2))
        if cand[-1] == cand_i_plus_1:
            i += 1
        if cand[-1] == cand_j_plus_sqrt2:
            j += 1
    return [a.val for a in cand]
```

##### Explanation:

The main idea for the algorithm above is still built off the fact that the \($$\small n + 1$$\)th value is the sum of 1 or $$\small \sqrt{2}$$ with a previous value.

Suppose we are storing the results in an array $$\small A$$. There are two entries of note to us: $$\small i$$, the smallest index such that $$\small A[i] + 1$$ &gt; $$\small A[-1]$$, and $$\small j$$, the smallest index such that $$\small A[j] + \sqrt{2} > A[-1]$$. These two entries could be the same, for example the second number. The idea is that until we have $$\small k$$ numbers, we take the smallest between the two indices, and then increment the one which contributed to the next smallest number. To illustrate, suppose $$\small A$$ is initialized to $$\small <0>$$, and $$\small i = j = 0$$. The computation proceeds as follows:

1. Since $$\small A[0] + 1 = 1 < A[0] + \sqrt{2} = 1.414$$, we push $$\small 1$$ into the $$\small A$$ and increment $$\small i$$. Now $$\small A = <0, 1>$$, $$\small i = 1, j = 0$$.
2. Since $$\small A[1] + 1 = 2 > A[0] + \sqrt{2} = 1.414$$, we push $$\small 1.414$$ into the $$\small A$$ and increment $$\small j$$. Now $$\small A = <0, 1, 1.414>$$, $$\small i = 1, j = 1$$.
3. Since $$\small A[1] + 1 = 2 < A[1] + \sqrt{2} = 2.414$$, we push $$\small 2$$ into the $$\small A$$ and increment $$\small i$$. Now $$\small A = <0, 1, 1.414, 2>$$, $$\small i = 2, j = 1$$.
4. Since $$\small A[2] + 1 = 2.414 = A[0] + \sqrt{2} = 2.414$$, we push $$\small 2.414$$ into the $$\small A$$ and increment both $$\small i$$ and $$\small j$$. Now $$\small A = <0, 1, 1.414, 2, 2.414>$$, $$\small i = 3, j = 2$$.
5. Since $$\small A[3] + 1 = 3 > A[2] + \sqrt{2} = 2.828$$, we push $$\small 2.828$$ into the $$\small A$$ and increment $$\small j$$. Now $$\small A = <0, 1, 1.414, 2, 2.414, 2.828>$$, $$\small i = 3, j = 3$$.
6. Since $$\small A[3] + 1 = 3 < A[0] + \sqrt{2} = 3.414$$, we push $$\small 3$$ into the $$\small A$$ and increment $$\small i$$. Now $$\small A = <0, 1, 1.414, 2, 2.414, 2.828, 3>$$, $$\small i = 4, j = 3$$.

The running time is now bounded by $$\small \mathcal O(n)$$. 



