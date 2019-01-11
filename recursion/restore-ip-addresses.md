#### Restore IP Addresses

> Given a string containing only digits, restore it by returning all possible valid IP address combinations.
>
> **Example:**
>
> ```
> Input: "25525511135"
>
> Output: ["255.255.11.135", "255.255.111.35"]
> ```

##### Recursion:

```py
def restoreIpAddresses(self, s):

    addrs = []

    def is_valid(a):
        if not a or len(a) > 3:
            return False
        if a[0] == '0' and len(a) > 1:
            return False
        if int(a) > 255:
            return False
        return True

    def helper(dots, cur, s):
        if not dots:
            if is_valid(s):
                cur.append(s)
                addrs.append(".".join(cur))
                cur.pop()
            return
        for i in range(1,4):
            if is_valid(s[:i]):
                cur.append(s[:i])
                helper(dots - 1, cur, s[i:])
                cur.pop()

    helper(3, [], s)
    return addrs
```

We generate all valid IP addresses via backtracking. Since no individual number can be greater than 255, we only need to consider chunks of up to length 3 at a time. If the chunk is valid, mark it down and try to break the rest of the string. Overall runtime is $$\small \mathcal O(1)$$. This is surprising at first, but there's two ways to think about it. There are a total of $$\small 2^{32} = 2564$$ total possible IP addresses. Each chunk can go up to e $$\small 255 = 2^{8}$$, so four chunks is $$\small \mathcal 2^{32}$$.

Another way to look at this is with the recurrence tree. The tree has a set height of 3, because there are only three dots to add. Therefore, the total number of leaves is limited. The total amount of work we do at each level is also limited, because we will at max check the validity of a 3 character string.  

