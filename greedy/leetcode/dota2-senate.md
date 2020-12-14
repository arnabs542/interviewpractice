#### Dota2 Senate

> In the world of Dota2, there are two parties: the Radiant and the Dire.
>
> The Dota2 senate consists of senators coming from two parties. Now the senate wants to make a decision about a change in the Dota2 game. The voting for this change is a round-based procedure. In each round, each senator can exercise one of the two rights:
>
> 1. Ban one senator's right: 
> A senator can make another senator lose all his rights in this and all the following rounds.
> 2. Announce the victory:
> If this senator found the senators who still have rights to vote are all from the same party, he can announce the victory and make the decision about the change in the game.
>
> Given a string representing each senator's party belonging. The character 'R' and 'D' represent the Radiant party and the Dire party respectively. Then if there are n senators, the size of the given string will be n.
>
> The round-based procedure starts from the first senator to the last senator in the given order. This procedure will last until the end of voting. All the senators who have lost their rights will be skipped during the procedure.
>
> Suppose every senator is smart enough and will play the best strategy for his own party, you need to predict which party will finally announce the victory and make the change in the Dota2 game. The output should be Radiant or Dire.
> 
> Example 1:
> ```
> Input: "RD"
> Output: "Radiant"
> Explanation: The first senator comes from Radiant and he can just ban the next senator's right in the round 1. 
> And the second senator can't exercise any rights any more since his right has been banned. 
> And in the round 2, the first senator can just announce the victory since he is the only guy in the senate who can vote.
> ```
> 
> Example 2:
> ```
> Input: "RDD"
> Output: "Dire"
> Explanation: 
> The first senator comes from Radiant and he can just ban the next senator's right in the round 1. 
> And the second senator can't exercise any rights anymore since his right has been banned. 
> And the third senator comes from Dire and he can ban the first senator's right in the round 1. 
> And in the round 2, the third senator can just announce the victory since he is the only guy in the senate who can vote.
> ```

##### Solution

This is a greedy solution - every member should try to use their vote this round if possible. However, they don't need to use it immediately. For example, if we encounter a senate like `DDDRR`, the first three `Dire` members will have 3 "floating" votes that can be used as soon as they come across a `Radiant` member. 

We need to preserve the relative ordering of the members, since later members are subject to he actions of early members. This need to preserve a FIFO ordering suggests using a queue.

We will put all senators in a queue as either `1`'s or `0`'s. As we come across each senator, we see if there is a floating ban on that party. If so, discard this senator. Otherwise, he contributes a floating ban to opposing party, and put him in the end of the queue for the next round. We end when all members of one party are banned. 

```py
def predict_party_victory(senate: "str") -> "str":
    queue = []
    floating_ban = [0, 0]
    party = [0, 0]

    # set up party and queue
    for s in senate:
        x = (s == 'R'): # Radiant members are assigned 1
        party[x] += 1
        queue.append(x)
    
    while all(party):
        member = part.popleft()
        if floating_ban[member]:
            floating_ban[member] -= 1   # member is voted out
        else:
            floating_ban[member^1] += 1
            queue.append(member)
    
    return 'Radiant' if party[1] else 'Dire'
```

Running time is $\small \mathcal O(n)$. While it may take multiple rounds to complete the simulation, each member can be voted out at least once. Therefore, the amount of work we do on each member is still constant.