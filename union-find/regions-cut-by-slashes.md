#### Regions Cut by Slashes

> In a N x N grid composed of 1 x 1 squares, each 1 x 1 square consists of a /, \, or blank space.  These characters divide the square into contiguous regions.
>
> \(Note that backslash characters are escaped, so a  is represented as "\".\)
>
> Return the number of regions.
>
> **Example 1:**
>
> ```
> Input
> [
>   " /",
>   "/ "
> ]
> Output: 2
> Explanation: 
> The 2x2 grid is as follows:
> ```
>
> ![](/assets/2x2_slash.png)
>
> Full description: [https://leetcode.com/problems/regions-cut-by-slashes/](https://leetcode.com/problems/regions-cut-by-slashes/)

##### Union-Find:

```py
def regionsBySlashes(grid):

    class DSU:
        def __init__(self, n):
            self.root = [i for i in range(n)]
            self.rank = [1 for _ in range(n)]

        def find(self, x):
            if self.root[x] != x:
                self.root[x] = self.find(self.root[x])
            return self.root[x]

        def union(self, x, y):
            xroot, yroot = self.find(x), self.find(y)
            if xroot == yroot:
                return False
            if self.rank[xroot] > self.rank[yroot]:
                self.root[yroot] = xroot
            elif self.rank[yroot] > self.rank[xroot]:
                self.root[xroot] = yroot
            else:
                self.root[xroot] = yroot
                self.rank[yroot] += 1
            return True

    N = len(grid)
    dsu = DSU(4*N*N)

    for r,row in enumerate(grid):
        for c,char in enumerate(row):
            block = 4*(r*N + c)

            # Connect components in each square 
            if char in "\ ":
                dsu.union(block + 0, block + 1)
                dsu.union(block + 3, block + 2)
            if char in " /":
                dsu.union(block + 0, block + 3)
                dsu.union(block + 2, block + 1)

            # connect to neighbors
            if r:
                dsu.union(block + 0, block - 4*N + 2)
            if c:
                dsu.union(block + 3, block - 4 + 1)

    return sum(dsu.find(x) == x for x in range(4*N*N))
```

The intuition behind the above algorithm is a bit difficult.  To find the number of components in a graph, we can use either depth-first search or union find, neither of which are hard to do. The main difficulty with this problem is in specifying the graph.

One "brute force" way to specify the graph is to associate each grid square with 4 nodes \(north, south, west, and east\), representing 4 triangles inside the square if it were to have both slashes. Then, we can connect all 4 nodes if the grid square is `" "`, and connect two pairs if the grid square is `"/"` or `""`. Finally, we can connect all neighboring nodes \(for example, the east node of the square at `grid[0][0]` connects with the west node of the square at `grid[0][1]`\).

Runtime complexity is bounded by $$\small \mathcal O(N∗N∗α(N))$$, where $$\small α$$ is the Inverse-Ackermann function since we join by rank.

