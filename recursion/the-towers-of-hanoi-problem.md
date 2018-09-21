#### The Towers of Hanoi Problem

> A peg contains rings in sorted order, with the largest ring being the lowest. You are to transfer these rings to another peg, which is initially empty. You have a third peg, which is initially empty. The only operation you can perform is taking a single ring from the top of one peg and placing it on the top of another peg. You must never place a larger ring above a smaller ring. Write a function which returns a sequence of operations that resultin the transfer of _n_ rings from one peg to another. Each operation should be encoded as a pair \(from\_peg, to\_peg\).

##### Code:

```py
def compute_tower_hanoi(num_rings):
    sequence = []

    def compute_tower_hanoi_helper(num_rings_to_move, from_peg, to_peg, temp_peg):
        if num_rings_to_move == 0:  # No rings left
            return
        # Recursive move the n-1 rings on top to temp_peg
        compute_tower_hanoi_helper(num_rings_to_move - 1, from_peg, temp_peg, to_peg)
        # Move the nth ring
        sequence.append((from_peg, to_peg))
        # Recursive move the n-1 rings from temp_peg to to_peg
        compute_tower_hanoi_helper(num_rings_to_move - 1, temp_peg, to_peg, from_peg)

    compute_tower_hanoi_helper(num_rings, 0, 1, 2)
    return sequence
```

##### Explanation:

This is a classic recursion problem, and it highlights the key properties of a recursive solution: do a little work, and pass the rest off. In the words of Jeff, let the recursion fairy do its thing!

The key idea of recursion is figuring out a base case, i.e. when to stop, and how to do the least amount of work before passing it on. In this case, if we are given _n_ rings to move, the least amount of work we can do is move at least 1 ring. Otherwise, nothing would get done, because nobody is doing any work at any time. So we agree to move one right, from the initial to peg to the final peg. But first, we ask the recursion fairy to move the top _n-1_ rings to the temp right so that we can legally move our ring, then we ask the recursion fairy to move the top _n-1_ rings back on top of ours.

