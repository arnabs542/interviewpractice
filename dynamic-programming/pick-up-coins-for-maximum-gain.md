#### Pick Up Coins for Maximum Gain

> In the pick-up-coins game, and even number of coins are placed in a line. Two players take turns at choosing one coin each - they can only choose from the two coins at the ends of the line. The game ends when all the coins have been picked up. The player whose coins have the higher total value wins. A player cannot pass his turn.
>
> Design an efficient algorithm for computing the maximum total value for the starting player in the pick-up-coins game.

##### Dynamic Programming

It's important to note that a greedy approach will not work for this problem. Suppose the coins are \[5, 25, 10, 1\]. If the first player chooses 5, the second player will choose 25, and thus win. The first player needs to choose 1 in order to force the second player to expose the 25, and thus lose the game.

