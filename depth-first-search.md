#### Remove Invalid Parentheses

> Remove the minimum number of invalid parentheses in order to make the input string valid. Return all possible results.
>
> **Note:** The input string may contain letters other than the parentheses`(`and`)`.
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

DFS \(Unoptimized\):

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



