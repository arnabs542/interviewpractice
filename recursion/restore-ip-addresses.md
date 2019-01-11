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



