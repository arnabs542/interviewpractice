#### Reveal Cards In Increasing Order

> In a deck of cards, every card has a unique integer.  You can order the deck in any order you want.
>
> Initially, all the cards start face down \(unrevealed\) in one deck.
>
> Now, you do the following steps repeatedly, until all cards are revealed:
>
> 1. Take the top card of the deck, reveal it, and take it out of the deck.
> 2. If there are still cards in the deck, put the next top card of the deck at the bottom of the deck.
> 3. If there are still unrevealed cards, go back to step 1.  Otherwise, stop.
>
> Return an ordering of the deck that would reveal the cards in **increasing order.**
>
> The first entry in the answer is considered to be the top of the deck.
>
> **Example 1:**
>
> ```
> Input: [17,13,11,2,3,5,7]
> Output: [2,13,3,11,5,17,7]
> Explanation: 
> We get the deck in the order [17,13,11,2,3,5,7] (this order doesn't matter), and reorder it.
> After reordering, the deck starts as [2,13,3,11,5,17,7], where 2 is the top of the deck.
> We reveal 2, and move 13 to the bottom.  The deck is now [3,11,5,17,7,13].
> We reveal 3, and move 11 to the bottom.  The deck is now [5,17,7,13,11].
> We reveal 5, and move 17 to the bottom.  The deck is now [7,13,11,17].
> We reveal 7, and move 13 to the bottom.  The deck is now [11,17,13].
> We reveal 11, and move 17 to the bottom.  The deck is now [13,17].
> We reveal 13, and move 17 to the bottom.  The deck is now [17].
> We reveal 17.
> Since all the cards revealed are in increasing order, the answer is correct.
> ```
>
> **Note:**
>
> 1. `1 <= A.length <= 1000`
> 2. `1 <= A[i] <= 10^6`
> 3. `A[i] != A[j]` for all `i != j`

##### Rebuild from Last Draw:

```py
def deckRevealedIncreasing(deck):

    deck.sort()
    res = []

    for i in reversed(range(len(deck))):
        if res:
            res = [deck[i]] + [res[-1]] + res[:-1]
        else:
            res = [deck[i]]

    return res
```

The idea of the algorithm is to work backwards from the last draw to the first and figure out what ordering works at each stage. For example, suppose the deck was `[1,2,3,4,5,6,7,8]`. We would want our last draw to be `[8]`, and the second last to be `[7,8]`. The third to last drawn card needs to be 6, and since the card directly after it gets moved to the end, the deck before drawing the 6 needs to be `[6,8,7]`. Then `[5,7,6,8]`, `[4,8,5,7,6]`, `[3,6,4,8,5,7]`, `[2,7,3,6,4,8,5]`, and finally `[1,5,2,7,3,6,4,8]`. The running time of this algorithm is $$\small \mathcal O(n^{2} \log{n})$$. There is an $$\small \mathcal O(n \log{n})$$ component from the initial sort, but each iteration we need to rebuild the array. We can remove the $$\small \mathcal O(n)$$ rebuild by simulating the indices directly.

##### Draw Simulation:

```py
def deckRevealedIncreasing(deck):

    index = collections.deque(range(len(deck)))
    ans = []

    while index:
        ans.append(index.popleft())
        if index:
            index.append(index.popleft())

    deck.sort()        
    res = [None] * len(deck)

    deck.sort()        
    for i,e in enumerate(ans):
        res[e] = deck[i]

    return res
```

The idea for the above algorithm is simply to draw the cards and see where they end up. The array  is the indices of the initial deck. Let the input be `[17,13,11,2,3,5,7]`. We will initialize `index` as `[0,1,2,3,4,5,6]` because there are 7 elements in the array. We then follow the rules for drawing until the deck is finished. The resulting `ans` array will be `[0, 2, 4, 6, 3, 1, 5]`. What this means is that the card at the 0th index originally will end up in the 0th index of the finished draw, the card at the 2nd index will end up at the 1st index, the card at the 4th index will end up at the 2nd index, etc. In turn, this means that we need to put the smallest element in the 0th index of the original deck, the 2nd smallest at the 2nd index, the 3rd smallest at the 4th index, etc. 

We can now rewrite the above algorithm to build the original deck as we process `index`, instead of having to do it in a separate pass:

```py
def deckRevealedIncreasing(deck):

    index = collections.deque(range(len(deck)))
    ans = [None] * len(deck)

    for card in sorted(deck):
        ans[index.popleft()] = card
        if index:
            index.append(index.popleft())

    return ans
```



