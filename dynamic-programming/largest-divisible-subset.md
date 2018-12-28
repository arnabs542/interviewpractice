#### Largest Divisible Subset

> Given a set of **distinct **positive integers, find the largest subset such that every pair \(Si, Sj\) of elements in this subset satisfies:
>
> Si% Sj= 0 or Sj% Si= 0.
>
> If there are multiple solutions, return any subset is fine.
>
> **Example 1:**
>
> ```
> Input: [1,2,3]
> Output: [1,2] (of course, [1,3] will also be ok)
> ```
>
> **Example 2:**
>
> ```
> Input: [1,2,4,8]
> Output: [1,2,4,8]
> ```

I initially thought this was a DSU type problem, but the difficulty in using a DSU would be figuring what a common root is for each group, since a number could be assigned to multiple subsets.

In reality, this is another disguised LIS problem. Suppose we have a subset $$\small S$$ which satisfies the condition. A new element $$\small E$$ would only be included in the subset if $$\small E {\% } max(S) == 0$$.

