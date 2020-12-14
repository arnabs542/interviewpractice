#### Course Schedule (Deadlock Detection)

> There are a total of `n` courses you have to take, labeled from `0` to `n-1`.
>
> Some courses may have prerequisites, for example to take course `0` you have to first take course `1`, which is expressed as a pair: `[0,1]`
>
> Given the total number of courses and a list of prerequisite pairs, is it possible for you to finish all courses?
> 
> Example 1:
> ```
> Input: 2, [[1,0]] 
> Output: true
> Explanation: There are a total of 2 courses to take. To take course 1 you should have finished course 0. So it is possible.
> ```
> Example 2:
> ```
> Input: 2, [[1,0],[0,1]]
> Output: false
> Explanation: There are a total of 2 courses to take. To take course 1 you should have finished course 0, and to take course 0 you should also have finished course 1. So it is impossible.
> ```

#### DFS
In an undirected graph, to look for a cycle we simply start on a node and perform a DFS. If at any point we come across a node we've already visited, then there is a cycle. 

For a directed cycle, such an approach isn't sufficient. Suppose we have an input like `[[1,0], [2,0], [2,1]]`. Course `1` requires course `0`, and course `2` requires both course `1` and `0`. If we simply perform a DFS on `2`, then we'll come across `0` twice and incorrectly report a cycle has been detected. 

Instead, we need to make a slight modification - we need to see if the already visited node is in our current path or if it was already processed. In other words, it's only a cycle if the node is currently in the recursion stack. 

```py
    def canFinish(numCourses: int, prerequisites: "List[List[int]]") -> bool:
        white = set(range(numCourses))
        gray, black = set(), set()
        
        graph = collections.defaultdict(list)
        for course, prereq in prerequisites:
            graph[prereq].append(course)
        
        def dfs(node, gray, black):
            if node in gray:
                return False
            if node in black:
                return True
            gray.add(node)
            for nei in graph[node]:
                if not dfs(nei, gray, black):
                    return False
            gray.discard(node)
            black.add(node)
            return True
        
        while white:
            node = white.pop()
            if not dfs(node, gray, black):
                return False
        
        return True
```

#### Topological Sort

Topological sort is built off of BFS and is more intuitive. We build a graph detailing the prereqs and their following courses, and well as the number of prereqs each course has. We start by looking at courses with no prereqs (if they exist), and as we pop them from the queue, we decrement the in-degree all their "children" courses by 1. Those that no longer have an in-degree means all of their prereqs have been finished, and they themselves can be taken. 

```py
def canFinish(numCourses: int, prerequisites: "List[List[int]]") -> bool:
    in_degrees = collections.defaultdict(int)
    children = collections.defaultdict(set)
    for course, prereq in prerequisites:
        in_degrees[prereq] += 1
        children[course].add(prereq)
    
    ready = collections.deque([])
    for n in range(numCourses):
        if n not in in_degrees:
            ready.append(n)
    
    while ready:
        course = ready.popleft()
        for child in children[course]:
            in_degrees[child] -= 1
            if in_degrees[child] == 0:
                ready.append(child)
                in_degrees.pop(child)
    
    return not in_degrees
```

Both solutions run in $\small \mathcal O(|E| + |V|)$. Space is bounded by $\small \mathcal O(|E|)$.