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

   Lets look at the non-constant case:
```
X' = A(t)*X
```

Where `A(t)` is the coefficient matrix and `X` is the vector of variables dependent on `t`.

Now lets take a case where the coefficient matrix `A` commutes with its antiderivative `B`.
```
A * B = B * A
```

Now, lets derivate the exponential of antiderivative with respect to `t`:
```
d/dt{ exp(B(t)) } = d/dt{ I + B + B^2/2 + B^3/6 + ... }
				  = 0 + A + AB/2 + BA/2 + AB^2/6 + BAB/6 + B^2A/6 + ...
				  = A + AB + AB^2/2 + ...
				  = A * exp(B)
```

We reached the above result only because `A` and `B` are commutative. Now, the comparing the above result with the system:
```
d/dt{ exp(B(t) } = A * B(t)
d/dt{ X(t) } = A * X(t)
```

Its easy to see that the solution for `X` is:
```
X(t) = B(t) C
```

Now, lets demonstrate the above with an example:
```
In [10]: eqs1                                                                                                                                                                                               
Out[10]: 
⎡d                         d                        ⎤
⎢──(f(r)) = r⋅g(r) + f(r), ──(g(r)) = -r⋅f(r) + g(r)⎥
⎣dr                        dr                       ⎦
```
For the above system, lets get the coefficient matrix `A(t)` using `linear_ode_to_matrix`
```
In [11]: (A1, A0), b = linear_ode_to_matrix(eqs1, [f(r), g(r)], r, 1)
In [26]: A0                                                                                                                                                                                                 
Out[26]: 
⎡1   r⎤
⎢     ⎥
⎣-r  1⎦
```

Now, we have the coefficient matrix `A(t)` as `A0`. Lets get the value for the commutative antiderivative:
```
In [27]: B = integrate(A0, r)                                                                                                                                                                               

In [28]: B                                                                                                                                                                                                  
Out[28]: 
⎡       2⎤
⎢      r ⎥
⎢ r    ──⎥
⎢      2 ⎥
⎢        ⎥
⎢  2     ⎥
⎢-r      ⎥
⎢────  r ⎥
⎣ 2      ⎦
```

We can get the solution easily by multiplying the exponential of the antiderivaitve with a vector of constants:
```
In [17]: C1, C2 = symbols("C1 C2")                                                                                                                                                            

In [18]: C = Matrix([C1, C2])

In [29]: sol_vector = exp(B) * C                                                                                                                                                                            

In [30]: sol = [Eq(f, s) for f, s in zip([f(r), g(r)], sol_vector)] 

In [31]: sol                                                                                                                                                                                                
Out[31]: 
⎡                ⎛ 2⎞            ⎛ 2⎞                    ⎛ 2⎞            ⎛ 2⎞⎤
⎢           r    ⎜r ⎟       r    ⎜r ⎟               r    ⎜r ⎟       r    ⎜r ⎟⎥
⎢f(r) = C₁⋅ℯ ⋅cos⎜──⎟ + C₂⋅ℯ ⋅sin⎜──⎟, g(r) = - C₁⋅ℯ ⋅sin⎜──⎟ + C₂⋅ℯ ⋅cos⎜──⎟⎥
⎣                ⎝2 ⎠            ⎝2 ⎠                    ⎝2 ⎠            ⎝2 ⎠⎦

```

We can verify the solution using `checksysodesol`:
```
In [32]: checksysodesol(eqs1, sol)                                                                                                                                                                          
Out[32]: (True, [0, 0])
```
   
   As you can see, along with handling the cases of the removed solvers like: ```_linear_2eq_order1_type3```, ```_linear_2eq_order1_type4```, ```_linear_2eq_order1_type5```, ```_linear_3eq_order1_type4``` this solver lets the library
   handle other cases as well which wasn't previously possible.
   
# Other tasks
1. Created a good first issue wherein some of the test cases from an old PR needed to be added in the master and ensured that 
   the new contributor gets a hands-on experience with contributing to the library.
2. Created and almost completed the PR for adding constant coefficient homogeneous solver that can efficiently handle. The details of which will be shared in the next week's blog post.

These were the tasks and implementations that I was able to work on and complete before I took a break for my exams since it was the question of
my graduation. I planned the month accordingly because if my exams were conducted, then even in that case,
I would have done adequate work for the Community Bonding Period. 
