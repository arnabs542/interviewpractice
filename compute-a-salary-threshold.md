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

Suppose there are $$\small n$$ employees. Let the array holding salary data be $$\small A$$. THe payrool, $$\small P(c)$$, implied by a cap of $$\small c$$, is $$\sum\_{n=1}^{\infty} 2^{-n} = 1$$



##### Code \(Iteration\):

```
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

We first sort the current salary array. We then keep two running sums: `unadjusted_salary_sum` and `adjusted_salary_sum`. 

The first one simply keeps a running sum of all the unadjusted salaries up to this point, while the second one calculates what the total salary looks like if we decide to cap everyone at the current salary. For example, given an original salary list of:

`[20, 30, 40, 90, 100]`

the adjusted caps would look like this:

`[100, 140, 170, 270, 280]`

The first number, 100, is derived by 5 \* 20, which is if we decide to cap everyone at 20. The 140 is derived by 20 + 30\*4, which is if we leave the first person as they are, and cap the rest of the employees. We see that at 90, our total salary goes to 270, which is over our limit. Therefore, our cap must be between 40, 90.

The second thing to realize is that if we have to cap any employee, the array will essentially be split into 2 sections: an uncapped section, and a capped section. Everything in the capped section will equal to the cap value, so that's why we can just take an average of:

 \(target\_payroll - unadjusted\_salary\_sum\)/adjusted\_people.



