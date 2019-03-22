#### Count the Number of Ways to Climb Stairs

> You are climbing stairs. You can advance 1 to $$\small k$$ steps at a time. Your destination is exactly $$\small n$$ steps up.
>
> Write a program which takes as inputs $$\small n$$ and $$\small k$$ and returns the number of ways in which you can get to your destination. For example, if $$\small n=4$$ and $$\small k=2$$, there are five ways in which to get to the destination:
>
> * four single stair advances,
> * two single stair advances followed by a double stair advance,
> * a single stair advance followed by a double stair advance followed by a single stair advance, 
> * a double stair advance followed by two single stairs advances, and 
> * two double stair advances

##### Dynamic Programming:

```py
def number_of_ways_to_top(top, maximum_step):

    ways = [0] * (top + 1)
    ways[0] = 1
    
    for i in range(len(ways)):
        for j in reversed(range(max(0, i-maximum_step), i)):
            ways[i] += ways[j]
    return ways[-1]
```

To get the the last step, we can take anywhere from $$\small 1 $$ to $$\small k$$ steps. Each of those steps can be reached by taking $$\small 1$$ to $$\small k$$ steps. We iterate through each step, working backwards to add up all the ways to get to the steps that can reach the current step in one turn. In a more formal sense, we can model the relationship as such: $$\small F(n,k) = \sum_{i=1}^{k} F(n-i,k)$$. Time complexity is $$\small \mathcal O(nk)$$, space complexity is $$\small \mathcal O(n)$$.

