#### Remove Invalid Parentheses

> Remove the minimum number of invalid parentheses in order to make the input string valid. Return all possible results.
>
> **Note:** The input string may contain letters other than the parentheses`(`and`)`.
>
> **Example 1:**
>
> ```
> Input: "()())()"
>
> Output: ["()()()", "(())()"]
> ```
>
> **Example 2:**
>
> ```
> Input: "(a)())()"
>
> Output: ["(a)()()", "(a())()"]
> ```
>
> **Example 3:**
>
> ```
> Input: ")("
>
> Output: [""]
> ```

##### DFS \(Unoptimized\):

```py
class Solution:
    def removeInvalidParentheses(self, s):
        self.ans = set()
        self.min_removed = float('inf')

        """
            index: current index in the original string 
            left_par: number of left parentheses in current expression
            right_par: number of right parentheses in current expression
            removed: number of characters removed
            cur: current expression
        """
        def dfs(index, left_par, right_par, removed, cur):
            # Reached end of string
            if index == len(s):

                # Check for validity and see if optimial solution needs to be update
                if left_par == right_par:
                    if removed < self.min_removed:
                        self.min_removed = removed
                        self.ans = {"".join(cur)}
                    elif removed == self.min_removed:
                        self.ans.add("".join(cur))
                return


            cur_char = s[index]
            # If the current character is not a parenthesis, just recurse one step ahead.
            if cur_char != '(' and cur_char != ")":
                cur.append(cur_char)
                dfs(index+1, left_par, right_par, removed, cur)
            else:
                # Else, one recursion is with ignoring the current character.
                # So, we increment the ignored counter and leave the left and right untouched.
                dfs(index+1, left_par, right_par, removed + 1, cur)

                # If the current parenthesis is an opening bracket, we consider it
                # and increment left and  move forward
                if cur_char == '(':
                    dfs(index+1, left_par + 1, right_par, removed, cur)

                # If the current parenthesis is a closing bracket, we consider it only if we
                # have more number of opening brackets and increment right and move forward.
                elif cur_char == ')' and right_par < left_par:
                    dfs(index+1, left_par, right_par + 1, removed, cur)

        dfs(0, 0, 0, 0, [])
        return list(self.ans)
```

Since we don't initially know which bracket can be removed, we simply generate all possible combinations and check them. This problem differs from the **Valid Parentheses** problem in that parentheses are either "\(" or "\)", so we don't need a stack to valid if an expression is valid. Instead, we simply need to remember how many open brackets and closed brackets we have, and if the two numbers match, then it is valid. Along the way, we make sure we never have more closed brackes than open brackets in our expression, since that would immediately invalidate it.

For each expression, we first see if it's valid, and if so, if it's longer than our previous best attempt. Running time is bounded by $$\small \mathcal O(2^{n})$$ due to generating all possible expressions. The rebuilding of the string at the end due to Python having immutable strings will actually increase the running time to $$\small \mathcal O(n*2^{n})$$.

**DFS \(Optimized\):**

```py
class Solution:
    def removeInvalidParentheses(self, s):
        
        self.ans = set()
        
        left_rem = right_rem = 0
        for p in s:
            if p == '(':
                left_rem += 1
            elif p == ')':
                if left_rem > 0:
                    left_rem -= 1
                else:
                    right_rem += 1
        
        def dfs(index, left, right, left_rem, right_rem, cur):
            if index == len(s):
                if left_rem == right_rem == 0:
                    self.ans.add("".join(cur))
                return
            
            cur_char = s[index]
            if cur_char == '(' and left_rem > 0:
                dfs(index + 1, left, right, left_rem - 1, right_rem, cur)
            if cur_char == ')' and right_rem > 0:
                dfs(index + 1, left, right, left_rem, right_rem - 1, cur)
            if cur_char != ')' and cur_char != '(':
                cur.append(cur_char)
                dfs(index + 1, left, right, left_rem, right_rem, cur)
                cur.pop()
            elif cur_char == '(':
                cur.append('(')
                dfs(index + 1, left + 1, right, left_rem, right_rem, cur)
                cur.pop()
            elif cur_char == ')' and right < left:
                cur.append(')')
                dfs(index + 1, left, right + 1, left_rem, right_rem, cur)
                cur.pop()
        
            
        dfs(0, 0, 0, left_rem, right_rem, [])
        return list(self.ans)
```



