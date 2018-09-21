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



