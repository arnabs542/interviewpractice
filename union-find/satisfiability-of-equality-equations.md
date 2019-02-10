#### Satisfiability of Equality Equations

> Given an array equations of strings that represent relationships between variables, each string `equations[i]` has length `4` and takes one of two different forms: `"a==b"` or `"a!=b"`.  Here, `a` and `b` are lowercase letters \(not necessarily different\) that represent one-letter variable names.
>
> Return `true` if and only if it is possible to assign integers to variable names so as to satisfy all the given equations.
>
> **Example 1:**
>
> ```
> Input: ["a==b","b!=a"]
> Output: false
> Explanation: If we assign say, a = 1 and b = 1, then the first equation is satisfied, but not the second.  
> There is no way to assign the variables to satisfy both equations.
> ```
>
> **Example 2:**
>
> ```
> Input: ["b==a","a==b"]
> Output: true
> Explanation: We could assign a = 1 and b = 1 to satisfy both equations.
> ```
>
> **Example 3:**
>
> ```
> Input: ["a==b","b==c","a==c"]
> Output: true
> ```
>
> **Example 4:**
>
> ```
> Input: ["a==b","b!=c","c==a"]
> Output: false
> ```
>
> **Example 5:**
>
> ```
> Input: ["c==c","b==d","x!=z"]
> Output: true
> ```

##### Union Find:

```py
def equationsPossible(self, equations: 'List[str]') -> 'bool':
    
    class UnionFind:

        def __init__(self):
            self.parents = [i for i in range(26)]
            self.rank = [1 for i in range(26)]

        def find_parent(self, x):
            if self.parents[x] == x:
                return self.parents[x]
            self.parents[x] = self.find_parent(self.parents[x])
            return self.parents[x]

        def union(self, x, y):
            x, y = ord(x) - ord('a'), ord(y) - ord('a')
            x_parent, y_parent = self.find_parent(x), self.find_parent(y)
            if x_parent == y_parent:
                return False
            if self.rank[x_parent] < self.rank[y_parent]:
                self.parents[x] = y_parent
            elif self.rank[y_parent] < self.rank[x_parent]:
                self.parents[y] = x_parent
            else:
                self.parents[y] = x_parent
                self.rank[x_parent] += 1
            return True

    uf = UnionFind()
    for eqn in equations:
        if eqn[1] == "=":
            uf.union(eqn[0], eqn[3])

    for eqn in equations:
        if eqn[1] == "!":
            if uf.find_parent(ord(eqn[0]) - ord('a')) == uf.find_parent(ord(eqn[3]) - ord('a')):
                return False
    return True
```

Suppose we are told that `a==b` and `b==c.` In this case, a,b,c should all be part of the same connected component. 

We go through all the equations first, looking for `==` statements. We then connect the two variables. Afterwards, we go through the list of equations again, looking for `!=` statements. If the two variables were previously assigned to the same connected component, then we know it's not possible to assign all the given operations. 

