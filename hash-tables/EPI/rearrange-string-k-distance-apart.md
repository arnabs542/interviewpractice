#### Rearrange String K Distance Apart

> Given a non-empty string s and an integer k, rearrange the string such that the same characters are at least distance k from each other.
>
> All input strings are given in lowercase letters. If it is not possible to rearrange the string, return an empty string "".
>
> Example 1: s = "aabbcc", k = 3 Result: "abcabc"
>
> The same letters are at least distance 3 from each other. Example 2: s = "aaabc", k = 3 Answer: ""
>
> It is not possible to rearrange the string. Example 3: s = "aaadbbcc", k = 2 Answer: "abacabcd" Another possible answer is: "abcabcda" The same letters are at least distance 2 from each other.

My initial attempt was to push all of the characters with their frequency into a heap and populate each element as I pop them. For each character, I would find the first empty index in the result array, and place the characters starting from that index k units apart. However, this solution fails for s = "aabbcc", k = 2, since after packing the a's and b's, there are not enough places to put the c's.

The main idea to solving this problem was through the realization that our final string must consists of "frames". For example, the highest frequency for the above string is 2, which means that we will need 2 frames of size at least 2 in order to complete the problem. Our frames would be $$\small <<a,b,c>,<a,b,c>>$$.

##### Code:

```py
def rearrangeString(string, k):

    cnts = [0] * 26;
    for c in string:
        cnts[ord(c) - ord('a')] += 1

    sorted_cnts = []
    for i in range(26):
        sorted_cnts.append((cnts[i], chr(i + ord('a'))))
    sorted_cnts.sort(reverse=True)

    max_cnt = sorted_cnts[0][0]
    blocks = [[] for _ in range(max_cnt)]
    i = 0
    for cnt in sorted_cnts:
        for _ in range(cnt[0]):
            blocks[i].append(cnt[1])
            i = (i + 1) % max(cnt[0], max_cnt - 1)

    for i in range(max_cnt-1):
        if len(blocks[i]) < k:
            return ""

    return "".join(map(lambda x : "".join(x), blocks))
```

##### Explanation:

We being by building a list of characters with their frequency, and then reverse sorting it so that the elements with the highest frequencies appear first. We then initialize as many frames as the highest frequency. We thing begin placing the characters into our frames. We want to pack all of the frames equally, except for the very last frame, because there is no need to ensure that it has a length of k since there is nothing following it. This is why we constantly modulo our index against the maximum of the frequency of the element and `max_cnt - 1`.

After placing all our elements, we then check to make sure our frames have the necessary minimum length. If any frame has less than k elements, then it means the elements in that frame and the elements in the next frame cannot be at least k distance apart, and therefore the problem is impossible. 

