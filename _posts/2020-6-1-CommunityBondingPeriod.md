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

# Blog Making

 Made my first blog post with jekyll. This took longer than expected since I had no experience with the same.
 
# Linear, first order, Constant Coefficient, homoegeous solver

The first type of solver in systems wherein any number of ODEs supplied will be solved by this solver. It does so by using matrix solutions.

Lets take the system below:
```
d/dt(X(t)) = A * X(t)
```

Now, from here on out, we will use the notation `X(t)` or simply `X` to represent a vector of dependent variables, variables dependent on the independent variable `t`. `A` will be used to denote a constant matrix and `A(t)` will be the non-constant one. 

We will write `d/dt(X(t))` as `X'` for simpler notations.

So, our system is:
```
X' = A*X
```

For example: 
```

In [38]: eqs                                                                                                                                                                                                
Out[38]: 
⎡d                       d              ⎤
⎢──(x(t)) = x(t) + y(t), ──(y(t)) = y(t)⎥
⎣dt                      dt             ⎦
```

The system above can be respresented in the matrix form with:
```
X(t) = [x(t), y(t)]
A = [ 1  1 ]
    [ 0  1 ]
```
Now, with this system, lets try to find the solution.

Given the system above, the general solution will be:
```
X = exp(A t) * C
```
Where `C` is the vector of constants. 

Computing matrix exponential directly can be very computationally expensive. Lets use another technique to achieve that.

We know that any matrix can be represented in its jordan form as(more information about jordan form [here](https://en.wikipedia.org/wiki/Jordan_normal_form)):
```
A = P * J * P-1
```
Hence, given the fact that exponential of `A` is:
```
exp(A) = P * exp(J) * P-1
```

We can easily get the exponential of `exp(A t)` as:
```
exp(A t) = P * exp(J t) * P-1
```
Now, computing exponential of jordan blocks is much simpler and computationally less expensive: details given [here](https://en.wikipedia.org/wiki/Matrix_exponential#General_case) and [here](https://www.math24.net/method-matrix-exponential/).

Hence, we use that to get the solution for the system by getting the value for `exp(A t)`.

Lets consider the example used above to show the validity of the solution:
```
In [41]: eqs                                                                                                                                                                                                
Out[41]: 
⎡d                       d              ⎤
⎢──(x(t)) = x(t) + y(t), ──(y(t)) = y(t)⎥
⎣dt                      dt             ⎦

In [46]: (A1, A0), b = linear_ode_to_matrix(eqs, [x(t), y(t)], t, 1)                                                                                                                                        

In [47]: A0                                                                                                                                                                                                 
Out[47]: 
⎡1  1⎤
⎢    ⎥
⎣0  1⎦

In [48]: A1                                                                                                                                                                                                 
Out[48]: 
⎡1  0⎤
⎢    ⎥
⎣0  1⎦
In [49]: P, J = A0.jordan_form()    

In [51]: C1, C2 = symbols("C1 C2")                                                                                                                                                            

In [52]: C = Matrix([C1, C2])                                                                                                                                                                               

In [53]: sol_vector = P * (J*t).exp() * P.inv() * C                                                                                                                                                         

In [54]: sol = [Eq(f, s) for f, s in zip([x(t), y(t)], sol_vector)]                                                                                                                                         

In [55]: sol                                                                                                                                                                                                
Out[55]: 
⎡           t         t             t⎤
⎣x(t) = C₁⋅ℯ  + C₂⋅t⋅ℯ , y(t) = C₂⋅ℯ ⎦

In [56]: checksysodesol(eqs, sol)                                                                                                                                                                           
Out[56]: (True, [0, 0])

In [58]: dsolve(eqs)                                                                                                                                                                                        
Out[58]: 
⎡                    t             t⎤
⎣x(t) = (C₁ + C₂⋅t)⋅ℯ , y(t) = C₂⋅ℯ ⎦

```

# Linear, n equations, Order 1, Type 3: non-constant coefficient homogeneous

   Worked on the non-constant coefficient homogeneous solver with the ondition that the coefficient matrix of the system of 
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
