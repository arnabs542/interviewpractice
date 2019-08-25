#### Team Photo Day - 1

> Design an algorithm that takes as input two teams and the heights of the players in the teams and checks if it is possible to take the photo subject to them placement constraint such that a player in the back row must be taller than the player in front of him. All players in a row must be from the same team.

##### Code:

```
class Team:
    Player = collections.namedtuple('Player', ('height'))

    def __init__(self, height):
        self._players = [Team.Player(h) for h in height]

    # Checks if team0 can be placed in front of team1.
    @staticmethod
    def valid_placement_exists(team0, team1):
        return all (a < b for a, b in zip(sorted(team0._players), sorted(team1._players)))
```

##### Explanation:

The brute force solution would be to consider every permutation of one array and compare it element by element against the other array. If there are $\small n$ players in each teach, then it would take $\small \mathcal O(n!)$ time to enumerate every possible permutation of a team, and testing a permutation takes $\small \mathcal O(n)$ time, resulting in $\small \mathcal O(n! * n)$ time, which is clearly unacceptable. 

The intuition to solving this problem is to compare extremes. Suppose that we want to place team B behind team A. If the tallest player in team B is not taller than the tallest player in team A, then the placement is not possible. If we can place the tallest players but if the second tallest player in team B is not taller than the second tallest player in team A, placement is once again impossible. We then extrapolate until we reach the shortest players. 

In other words, we simply sort the two arrays, then compare the elements in the same indices to ensure that every player in team0 is shorter than his counterpart in team1. 

Running time is dominated by sorting, i.e., $\small \mathcal O(n \log {n})$.

