#### Friend Recommendation

> You are given a network of friends from LinkedIn or Facebook as an array. Each element of this array is a pair `[idX, idY]` which means user `X` and user `Y` are friends. You also given the id of the current user. Find the person who
>
> * Is not your friend.
> * Has the most common friends with you.
>
> Example 1:
> ```
> Input: friends = [[1, 2], [1, 3], [2, 4], [2, 5], [3, 4]], id = 1
> Output: 4
> Explanation:
> User 4 has 2 (2, 3) common friends with user 1.
> User 5 has 1 (3) common friend with user 1.
> So return 4.
> ```

##### Solution

We should begin by building network - we can use an adjacency list for this. Afterwords, we can iterate through the list, and compare the length of the overlap of the friend groups for every person that's not already a friend of the current user. It takes $\small \mathcal O(n)$ time to build the list, $\small \mathcal O(u^{2})$ time to compare the pairs of users, and $\small \mathcal O(l)$ time to calculate the intersection between two lists.

While this while give us the correct solution, we can improve this solution to linear time. After building the network, we can use the original connections themselves to figure out who to recommend.

For each connection `[idX, idY]`, user `idX` should receive a point if he is not already a friend, and user`idY` is a friend of the current user. 

```py
def friend_recommendation(connections, user_id):
    graph = collections.defaultdict(set)
    for x, y in graph:
        graph[x].add(y)
        graph[y].add(x)
    
    points = collections.Counter()
    for x, y in graph:
        points[x] += (x not in graph[user_id]) and (y in graph[user_id])
        points[y] += (y not in graph[user_id] and (x in graph[user_id]))
    
    return max(points.items(), key = lambda x: x[1])[0]