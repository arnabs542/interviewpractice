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

As we can see from the first splitting, a greedy approach is not always optimal. Furthermore, we cannot just trivially extend the optimum placement for the first $$\small i-1$$ words to an optimum placement for the $$\small i$$th worth. This is because by introducing the $$\small i$$$$\small i$$th word, we may need to adjust the previous arrangement. In other words, the optimum placement for the first $$\small i$$ words is not always an optimum placement for the first $$\small i-1$$ words. However, what we can say is that if in the optimum placement for the $$\small i$$th word the last line consists of words $$\small j, j+1,...,i$$, then in this placement, the first $$\small j-1$$ words must be placed optimally.

