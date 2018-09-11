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

First we check if a cap is even necessary by summing up current salaries and comparing it against the target payroll. If it's not necessary, we just return a -1. 

The possible cap space is from 0 to the highest salary, otherwise there is no need to cap. We binary binary search on the search space, each time seeing what our total payroll would be if we chose that particular cap. Running time should be bounded by $$\small \mathcal O(n \log{n})$$, the logarithmic part coming from the binary search, and the linear portion coming from the summing after choosing a bound. This is also the bound for the initial sorting.

