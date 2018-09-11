#### Compute a Salary Threshold

> You are working in the finance office for ABC corporation. ABC needs to cut payroll expenses to a specified target. The chief executive officer wants to do this by putting a cap on last year's salaries. Every employee who earned more than the cap last year will be paid the cap this year; employees who earned no more than the cap will see no change in their salary.
>
> For example, if there were five employees with salaries $90, $30, $100, $40, and $20, and the target payroll this year is $210, then 60 is a suitable salary cap, since 60 + 30 + 60 + 40 + 20 = 210.
>
> Design an algorithm for computing the salary cap, given existing salaries and the target payroll.

##### Code \(Trial-and-Error\):

```py
def find_salary_cap(target_payroll, current_salaries):

    if target_payroll > sum(current_salaries):
        return -1

    def find_index(x, salaries):
        lo, hi = 0, len(salaries)
        while lo < hi:
            mid = (lo + hi) >> 1
            if salaries[mid] < x:
                lo = mid + 1
            else:
                hi = mid
        return lo

    current_salaries.sort()
    lo, hi = 0, current_salaries[-1]
    cap = 0.0
    while not math.isclose(lo, hi):
        mid = (lo + hi) / 2
        idx = find_index(mid, current_salaries)
        total_pay = sum(current_salaries[:idx]) + mid * (len(current_salaries) - idx)
        if total_pay <= target_payroll:
            cap = mid
            lo = mid
        else:
            hi = mid
    return cap
```

##### Explanation:

The cap, if it exists, lies between 0 and the maximum current salary. The payroll increases with the cap, which suggests using binary search in this range - if a cap is too high, no higher cap will work; the same is true if the cap is too low.

Suppose there are $$\small n$$ employees. Let the array holding salary data be $$\small A$$. The payroll, $$\small P(c)$$, implied by a cap of $$\small c$$, is $$\small \sum_{i=0}^{n-1} min(A[i], c)$$. Each step of the binary search requires evaluating $$\small P(c)$$ which takes time $$\small \mathcal O(n)$$. The number of binary search steps depends on the largest salary and the desired accuracy.

##### Code \(Iteration\):

```py
def find_salary_cap(target_payroll, current_salaries):
    current_salaries.sort()
    unadjusted_salary_sum = 0
    for i, current_salary in enumerate(current_salaries):
        adjusted_people = len(current_salaries) - i
        # Total salary if we capped all people to current salary
        adjusted_salary_sum = adjusted_people * current_salary
        if adjusted_salary_sum + unadjusted_salary_sum >= target_payroll:
            return (target_payroll - unadjusted_salary_sum)/adjusted_people
        unadjusted_salary_sum += current_salary
    return -1.0
```

##### Explanation:

We can use a slightly more analytical method to avoid the need for a specified tolerance. The intuition is that as we increase the cap, as long as it does not exceed someone's salary, the payroll increases linearly. This is because each time the cap goes over a salary, we will see a big jump in total payroll from that salary. instead of incremental changes due to the cap adjusting. 

Assume the salaries are given by an array $$\small A$$, which is sorted. Suppose the cap for a total payroll of $$\small T$$ is known to lie between the $$\small k$$th and \($$\small k$$ + 1\)th salaries. We want $$\small \sum_{i=0}^{k-1} A[i] + (n-k)c$$ to equal = $$\small T$$, which solves to $$\small c = (T - \sum_{i=0}^{k-1} A[i])/(n-k)$$. 

For example, suppose $$\small A = <20,30,40,90,100>$$, and $$\small T = 210$$. The payrolls for caps equal to the salaries in $$\small A$$ are $$\small <100,140,170,270,280>$$. Since $$\small T= 210$$ lies between 170 and 270, the cap lies between 40 and 90. For any cap $$\small c$$ between 40 and 90, the implied payroll is 20 + 30 + 40 + 2$$\small c$$. We want this to be 210, so we solve 20 + 30 + 40 + 2$$\small c$$ = 210 for $$\small c$$, yielding $$\small c = 60$$.

In other words, if we have to cap any employee, the array will essentially be split into 2 sections: an uncapped section, and a capped section. Everything in the capped section will equal to the cap value, so that's why we can just take an average of:

\(target\_payroll - unadjusted\_salary\_sum\)/adjusted\_people



