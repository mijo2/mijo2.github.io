---
layout: post
title: GSoC'20 - Community Bonding Period
---

I was overwhelmed with joy on May 4 when I saw my name in the list of selected students in the GSoC list of selected students
for SymPy. I went through the process and made my first blog post which detailed my journey with SymPy till I got selected.
In May, I worked for roughly three weeks since my college declared open book exams for final year students, and
 given these were my final exams, I took a break of two weeks starting from May 24. I informed my mentor about this and that I
would work harder when my exams end.

Below is the list of tasks that I completed for the Community Bonding Period:

1. Made my first blog post with jekyll.
2. Worked on the non-constant coefficient homogeneous solver with the condition that the coefficient matrix of the system of 
   ODEs is commutative with its anti-derivative. The PR regarding this was merged in this period. Hence, two of the nine tasks
   that are to be completed for this project have been done successfully.
   
   Now the library can handle lots of systems of ODEs that were not possible before, along with the fact that lots of old code 
   and solvers were removed with this PR. The solver is named ```_linear_neq_order1_type3```. For example:
   
   ```
   In [1]: from sympy import *                                                                                                                                                                                 
   In [2]: from sympy.abc import *                                                                                                                                                                             
   In [3]: f, g, h = symbols("f g h", cls=Function)                                                                                                                                                            
   In [4]: eqs4 = [Eq(f(x).diff(x), x*(f(x) + g(x) + h(x))), Eq(g(x).diff(x), x*(f(x) + g(x) + h(x))), Eq(h(x).diff(x), x*(f(x) + g(x) + h(x)))]                                                               

   In [6]: dsolve(eqs4)                                                                                                                                                                                        
   Out[6]: 
   ⎡                                           2                                               2                                               2⎤
   ⎢                                        3⋅x                                             3⋅x                                             3⋅x ⎥
   ⎢                                        ────                                            ────                                            ────⎥
   ⎢       2⋅C₁   C₂   C₃   ⎛C₁   C₂   C₃⎞   2             C₁   2⋅C₂   C₃   ⎛C₁   C₂   C₃⎞   2             C₁   C₂   2⋅C₃   ⎛C₁   C₂   C₃⎞   2  ⎥
   ⎢f(x) = ──── - ── - ── + ⎜── + ── + ──⎟⋅ℯ    , g(x) = - ── + ──── - ── + ⎜── + ── + ──⎟⋅ℯ    , h(x) = - ── - ── + ──── + ⎜── + ── + ──⎟⋅ℯ    ⎥
   ⎣        3     3    3    ⎝3    3    3 ⎠                 3     3     3    ⎝3    3    3 ⎠                 3    3     3     ⎝3    3    3 ⎠      ⎦

   ```
   
   As you can see, along with handling the cases of the removed solvers like: ```_linear_2eq_order1_type3```, ```_linear_2eq_order1_type4```, ```_linear_2eq_order1_type5```, ```_linear_3eq_order1_type4``` this solver lets the library
   handle other cases, as demonstrated in the example above, i.e. when the coefficient matrix of a linear order 1 homogeneous
   system of ODEs has all the elements as the same, as in every coefficient is equal, then regardless of the number of equations
   in the system of ODEs, this solver will be able to handle it.
3. Created a good first issue wherein some of the test cases from an old PR needed to be added in the master and ensured that 
   the new contributor gets a hands-on experience with contributing to the library.
4. Created and almost completed the PR for adding constant coefficient homogeneous solver that can efficiently handle.

These were the tasks that I was able to work on and complete before I took a break for my exams since it was the question of
my graduation. I planned the month accordingly because if my exams were conducted, then even in that case,
I would have done adequate work for the Community Bonding Period. 
