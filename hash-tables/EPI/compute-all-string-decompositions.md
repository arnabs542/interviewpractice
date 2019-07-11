#### Compute all String Decompositions

> Write a program which takes as inputs a string \(the "sentence"\) and an array of strings \(the "words"\), and returns the starting indices of substrings of the sentece string which are the concatenation of all the strings in the words array. Each string must appear exactly once, and their ordering is immaterial. Assuming all strings in the words array have equal length. It is possible for the words array to contain duplicates.
>
> ```
> sentence = "amanaplanacanal", words = ["can", "apl", "ana"]
>
> "aplanacan" is a substring of the sentence that is the concatenation of all words.
> ```

##### Code \(Unoptimized\):

```py
def find_all_substrings(s, words):
    char_freq = collections.Counter("".join(words))
    word_freq = collections.Counter(words)
    cur_sub_char_freq = collections.defaultdict(int)

    word_length = len(words[0])
    sub_length = word_length * len(words)

    res = []

    for i, c in enumerate(s):
        cur_sub_char_freq[c] += 1
        if i >= sub_length:
            cur_sub_char_freq[s[i-sub_length]] -= 1
            if cur_sub_char_freq[s[i-sub_length]] == 0:
                cur_sub_char_freq.pop(s[i-sub_length])
        if cur_sub_char_freq == char_freq:
            cur_sub_word_freq = collections.defaultdict(int)
            for j in range(i - sub_length + 1, i+1, word_length):
                cur_sub_word_freq[s[j:j+word_length]] += 1
            if cur_sub_word_freq == word_freq:
                res.append(i - sub_length + 1)
    return res
```

##### Explanation:

The fact that all words are of the same length greatly simplifies the problem. The above approach heavily utilizes this constraint. We start by generate a frequency mapping of all the characters in our words array and a frequency mapping of all the words in the words array. We then move a sliding window of the correct size through the sentence. Whenever we find a substring that matches the character frequency, we then check to see if it is an actual match by decomposing all the words. However, this approach is not ideal because we have iterate through every substring, and in the event that we get a frequency match for every substring, our time complexity is bounded by $$\small \mathcal O(n^{2})$$ \(suppose substring length = N/2, where N is the length of the sentence\). Furthermore, comparing hash tables takes $$\small \mathcal O(n)$$ time, since we have to iterate through all keys in the hash table.

##### Code \(Book\):

```py
def find_all_substrings(s, words):
    def match_all_words_in_dict(start):
        curr_string_to_freq = collections.Counter()
        for i in range(start, start + len(words) * unit_size, unit_size):
            curr_word = s[i:i + unit_size]
            it = word_to_freq[curr_word]
            if it == 0:
                return False
            curr_string_to_freq[curr_word] += 1
            if curr_string_to_freq[curr_word] > it:
                # curr_word occurs too many times for a match to be possible
                return False
        return True
    word_to_freq = collections.Counter(words)
    unit_size = len(words[0])
    return [i for i in range(len(s) - unit_size * len(words) + 1) if match_all_words_in_dict(i)]
```

##### Explanation:

The book code is pretty much the same as mine, with some changes in implementation. Again, we move a sliding window of the correct size through the sentence, and then we try to decompose the substring into words.

The time complexity analysis can be derived as follows: Let $$\small m$$ be the number of words and $$\small n$$ the length of each word. Let $$\small N$$ be the length of the sentence. For any fixed $$\small i$$, to check if the string of length $$\small nm$$ starting at an offset of $$\small i$$ in the sentence is the concatenation of all words has time complexity $$\small \mathcal O(nm)$$, assuming a has table is used to store the set of words. This implies the overall time complexity is $$\small \mathcal O(Nnm)$$. In practice, the individual checks should be slightly faster because of early terminations.

