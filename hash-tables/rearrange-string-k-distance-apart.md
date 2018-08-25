Code:

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



