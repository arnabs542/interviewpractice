#### Is an Anonymous Letter Constructible

> Write a program which takes text for an anonymous letter and text for a magazine and determines if it is possible to write the anonymous letter using the magazine. THe anonymous letter can be written using the magazine if for each character in the anonymous letter, the number of times it appears in the anonymous letter is no more than the number of times it appears in the magazine.

##### Code:

```py
def is_letter_constructible_from_magazine(letter_text, magazine_text):
    magazine = collections.Counter(magazine_text)
    for c in letter_text:
        if c not in magazine or magazine[c] <= 0:
            return False
        magazine[c] -= 1
    return True
```

##### Explanation:

Again, a rather straightforward application of hash tables. We simply need to ensure that frequency each character appears in the letter is less than equal to the frequency the character appears in the magazine. 

If the character count for the letter is much shorter than that of the magazine, we could instead build a hash table of the letter instead to save some space. 

```py
def is_letter_constructible_from_magazine(letter_text, magazine_text):

    # Compute the frequencies for all chars in letter_text.
    char_frequency_for_letter = collections.Counter(letter_text)

    # Checks if characters in magazine_text can cover characters in
    # char_frequency_for_letter.
    for c in magazine_text:
        if c in char_frequency_for_letter:
            char_frequency_for_letter[c] -= 1
            if char_frequency_for_letter[c] == 0:
                del char_frequency_for_letter[c]
                if not char_frequency_for_letter:
                    # All characters for letter_text are matched.
                    return True

    # Empty char_frequency_for_letter means every char in letter_text can be
    # covered by a character in magazine_text.
    return not char_frequency_for_letter
```

Overall runtime is $\small O(m+n)$, where $\small m,n$ are the lengths of the letter and magazine, respectively. Space depends on which text we choose to build the character for. 

