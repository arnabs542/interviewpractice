#### The Pretty Printing Problem

> Consider the problem of laying out texting using a fixed width font. Each line can hold no more than a fixed number of characters. Words on a line are to be separated by exactly one blank. Therefore, we may be left with whitespace at the end of a line \(since the next word will not fit in the remaining space\). This whitespace is visually unappealing.
>
> Define the _messiness_ of the end-of-line whitespace as follows. The messiness of a single line ending with $$\small b$$ blank characters is $$\small b^{2}$$. The total messiness of a sequence of lines is the sum of the messiness of all the lines. A sequence of words can be split across lines in different with different messiness:
>
> ```
> I have inserted a large number of ⎵⎵⎵
> new examples from papers for the ⎵⎵⎵⎵
> Mathematical Tripos during the last ⎵
> years, which should be useful to ⎵⎵⎵⎵
> Cambridge students. ⎵⎵⎵⎵⎵⎵⎵⎵⎵⎵⎵⎵⎵⎵⎵⎵
>
> Messiness = 3^2 + 4^2 + 1^2 + 4^2 + 16^2 = 298 
>
> I have inserted a large number of ⎵⎵⎵
> new examples from papers for ⎵⎵⎵⎵⎵⎵⎵
> the Mathematical Tripos during ⎵⎵⎵⎵⎵
> the last years, which should be ⎵⎵⎵⎵
> useful to Cambridge students. ⎵⎵⎵⎵⎵⎵
>
> Messiness = 3^2 + 7^2 + 5^2 + 4^2 + 6^2 = 132
> ```
>
> Given text, i.e., a string of words separated by single blanks, decompose the text into lines such that no word is split across lines and the messiness of the decomposition is minimized. Each line can hold no more than a specified number of characters.

##### Dynamic Programming:

As we can see from the first splitting, a greedy approach is not always optimal. Furthermore, we cannot just trivially extend the optimum placement for the first $$\small i-1$$ words to an optimum placement for the $$\small i$$th worth. This is because by introducing the $$\small i$$$$\small i$$th word, we may need to adjust the previous arrangement. In other words, the optimum placement for the first $$\small i$$ words is not always an optimum placement for the first $$\small i-1$$ words. However, what we can say is that if in the optimum placement for the $$\small i$$th word the last line consists of words $$\small j, j+1,...,i$$, then in this placement, the first $$\small j-1$$ words must be placed optimally. Thus we have our recurrence relationship: $$\small \text{min}_{j \le i} f(j,i) + M(j-1)$$, where $$\small f(j,i)$$ is the messiness of a single line consisting of words $$\small j$$ to $$\small i$$ inclusive and $$\small M(i)$$ is the minimum messiness when placing the first $$\small i$$ words.

```py
def minimum_messiness(words, line_length):
    num_remaining_blanks = line_length - len(words[0])
    # min_messiness[i] is the minimum messiness when placing words[0:i + 1]
    min_messiness = [num_remaining_blanks**2] + [float('inf') * (len(words) - 1)]

    for i in range(1, len(words)):
        num_remaining_blanks = line_length - len(words[i])
        min_messiness[i] = min_messiness[i - 1] + num_remaining_blanks**2
        # Try adding words[i - 1], words[i - 2], ...
        for j in reversed(range(i)):
            num_remaining_blanks -= len(words[j]) + 1
            if num_remaining_blanks < 0:
                # Not enough space to add more words
                break
            first_j_messiness = 0 if j - 1 < 0 else min_messiness[j - 1]
            current_line_messiness = num_remaining_blanks ** 2
            min_messiness[i] = min(min_messiness[i], first_j_messiness + current_line_messiness)
    return min_messiness[-1]
```

In the above algorithm, the array `min_messiness[i]` stores the minimum messiness required to place the first $$\small i$$ words. As we encounter a new word, we work backwards to figure out the optimal subset of words, including the new word, we can fit on the last line.

Let $$\small L$$ be the line length. Then there can certainly be no more than $$\small L$$ words on a line, so the amount of time spent processing each word is $$\small \mathcal O(L)$$. Therefore, if there are $$\small n$$ words, the time complexity is $$\small \mathcal O(nL)$$. The space complexity is $$\small \mathcal O(n)$$ for the cache. 

