---
layout: post
title: GSoC'20 - Final Report
---

This report is the summary of the work that I have done in GSoC'20 for the project "**Improving and Extending ODE module**". For detailed progress report and theory behind many of the functionalities added in this project, please check the wiki page by my mentor [Oscar Benjamin](https://github.com/oscarbenjamin) named [ODE Systems roadmap](https://github.com/sympy/sympy/wiki/ODE-Systems-roadmap) which details the theory for most of the methods implemented, my [GSoC proposal](https://docs.google.com/document/d/1L7WgLGU_4oKfOivEEFk93MKbueNEZj2_IWFqUsEJmgQ/edit?usp=sharing) which details the theory in latex format,  and you can also check out my weekly [blog posts](https://mijo2.github.io/) that details the theory of the functionalities implemented with examples. The link to tracker issue for this project is [here](https://github.com/sympy/sympy/issues/19178)

# About me
My name is [Milan Jolly](https://github.com/mijo2) and I am a Computer Science and Engineering graduate from Indian Institute of Technology, Patna. I graduated this year.

# Project Outline
The overall work for the project was divided into roughly four parts, depending on the periods:

**Community Bonding period**: 
1. [#18720](https://github.com/sympy/sympy/pull/18720): Added the linear, first order, constant coefficient, homogeneous solver along with the outline for the new file `systems.py`. It was decided that all the code pertaining to solving systems of ODEs will be added in this file.
2. [#19185](https://github.com/sympy/sympy/pull/19185): Added the linear, first order, non-constant coefficient with commutative antiderivative, homogeneous solver.

**Phase 1**:
1. [#19341](https://github.com/sympy/sympy/pull/19341): Added linear, first order, constant coefficient non-homogeneous solver along with some work related to simplifying the solutions.
2. [#19594](https://github.com/sympy/sympy/pull/19594): Added the linear, first order, non-constant coefficient with commutative antiderivative, non-homogeneous solver.

**Phase 2**:
1. [#19653](https://github.com/sympy/sympy/pull/19653): Combined the four solvers added into one function named `linodesolve` that can solve linear first order systems when the appropriate coefficient matrices are passed. Along with that, support for any system that can be solved for the highest order terms to get more linear systems was added.
2. [#19695](https://github.com/sympy/sympy/pull/19695) : Added `dsolve_system`function along with the necessary helper functions required for adding the component division and higher order to lower order reduction techniques.
3. [#19733](https://github.com/sympy/sympy/pull/19733): Some miscellaneous yet important tasks like adding `checksysodesol`call to `checkodesol`, extending `constants_renumber` for handling systems of ODEs, and changing the output format of `linear_ode_to_matrix`. These tasks were important to be carried out for the future PRs in the remaining time of Phase 2 and Phase 3.
4. [#19762](https://github.com/sympy/sympy/pull/19762): Adding the functionality to divide the systems of ODEs into sub-systems. This lets the module tackle types of systems wherein a particular solver for the whole system isn't available but if the sub-systems can be solved separately using different solvers, then the answer for the entire system can be obtained. More details on the roadmap, proposal or my blog post's Week 7 page.

**Phase 3**:
1. [#19838](https://github.com/sympy/sympy/pull/19838): Added different techniques to reduce systems of higher order to first order. This PR added a total of 5 ways to reduce higher order systems to first order or solvable systems. Two of these four ways work only for the second order systems. These ways aren't limited by the number of equations in the system. Along with that, two new different linear first order solvers were added for faster computation.

 # Issues created
 I opened two good first issues for better help future contributors to start their journey with SymPy knowing how important easy issues are for starting the open source journey. Below is the list of issues that I have opened for future contributors:
 
 [#19312](https://github.com/sympy/sympy/issues/19312)
 [#19763](https://github.com/sympy/sympy/issues/19763)

# Examples
There are a lot of types of systems that can be now solved by the module that wasn't possible before this project. I would list the systems below to show how much the SymPy's capability to solve systems of ODEs has matured.

## Linear, first order systems 
Many of the old solvers were able to handle only 2 or 3 systems at a time. There was only one n equations(denoting any number of equations) solver which was very limited as it required the system to be homogeneous and the coefficient matrix to be diagonalizable. These constraints were effectively removed by the new solvers with only one constraint that the system have coefficient matrix with commutative antiderivative and if not, it can be divided into sub-systems that have coefficient matrices with commutative antiderivatives.
```
In [18]: sum_terms = f(t) + g(t) + h(t) + k(t)                                                                                                                                                              

In [19]: eqs = [Eq(f.diff(t), sum_terms) for f in [f(t), g(t), h(t), k(t)]]                                                                                                                                 

In [20]: eqs                                                                                                                                                                                                
Out[20]: 
[Eq(Derivative(f(t), t), f(t) + g(t) + h(t) + k(t)),
 Eq(Derivative(g(t), t), f(t) + g(t) + h(t) + k(t)),
 Eq(Derivative(h(t), t), f(t) + g(t) + h(t) + k(t)),
 Eq(Derivative(k(t), t), f(t) + g(t) + h(t) + k(t))]

In [21]: dsolve_system(eqs)                                                                                                                                                                                 
Out[21]: 
[[Eq(f(t), -C1 - C2 - C3 + C4*exp(4*t)),
  Eq(g(t), C1 + C4*exp(4*t)),
  Eq(h(t), C2 + C4*exp(4*t)),
  Eq(k(t), C3 + C4*exp(4*t))]]

In [22]: sol = _[0]                                                                                                                                                                                         

In [23]: checksysodesol(eqs, sol)                                                                                                                                                                           
Out[23]: (True, [0, 0, 0, 0])
In [18]: sum_terms = f(t) + g(t) + h(t) + k(t)                                                                                                                                                              

In [19]: eqs = [Eq(f.diff(t), sum_terms) for f in [f(t), g(t), h(t), k(t)]]                                                                                                                                 

In [20]: eqs                                                                                                                                                                                                
Out[20]: 
[Eq(Derivative(f(t), t), f(t) + g(t) + h(t) + k(t)),
 Eq(Derivative(g(t), t), f(t) + g(t) + h(t) + k(t)),
 Eq(Derivative(h(t), t), f(t) + g(t) + h(t) + k(t)),
 Eq(Derivative(k(t), t), f(t) + g(t) + h(t) + k(t))]

In [21]: dsolve_system(eqs)                                                                                                                                                                                 
Out[21]: 
[[Eq(f(t), -C1 - C2 - C3 + C4*exp(4*t)),
  Eq(g(t), C1 + C4*exp(4*t)),
  Eq(h(t), C2 + C4*exp(4*t)),
  Eq(k(t), C3 + C4*exp(4*t))]]

In [22]: sol = _[0]                                                                                                                                                                                         

In [23]: checksysodesol(eqs, sol)                                                                                                                                                                           
Out[23]: (True, [0, 0, 0, 0])

In [26]: eqs = [Eq(f.diff(t), t*sum_terms + t**2) for f in [f(t), g(t), h(t), k(t)]]                                                                                                                        

In [27]: eqs                                                                                                                                                                                                
Out[27]: 
[Eq(Derivative(f(t), t), t**2 + t*(f(t) + g(t) + h(t) + k(t))),
 Eq(Derivative(g(t), t), t**2 + t*(f(t) + g(t) + h(t) + k(t))),
 Eq(Derivative(h(t), t), t**2 + t*(f(t) + g(t) + h(t) + k(t))),
 Eq(Derivative(k(t), t), t**2 + t*(f(t) + g(t) + h(t) + k(t)))]

In [28]: dsolve_system(eqs)                                                                                                                                                                                 
Out[28]: 
[[Eq(f(t), C1*exp(2*t**2)/4 - C1/4 + C2*exp(2*t**2)/4 - C2/4 + C3*exp(2*t**2)/4 - C3/4 + C4*exp(2*t**2)/4 + 3*C4/4 + exp(2*t**2)*Integral(t**2*exp(-2*t**2), t)),
  Eq(g(t), C1*exp(2*t**2)/4 + 3*C1/4 + C2*exp(2*t**2)/4 - C2/4 + C3*exp(2*t**2)/4 - C3/4 + C4*exp(2*t**2)/4 - C4/4 + exp(2*t**2)*Integral(t**2*exp(-2*t**2), t)),
  Eq(h(t), C1*exp(2*t**2)/4 - C1/4 + C2*exp(2*t**2)/4 + 3*C2/4 + C3*exp(2*t**2)/4 - C3/4 + C4*exp(2*t**2)/4 - C4/4 + exp(2*t**2)*Integral(t**2*exp(-2*t**2), t)),
  Eq(k(t), C1*exp(2*t**2)/4 - C1/4 + C2*exp(2*t**2)/4 - C2/4 + C3*exp(2*t**2)/4 + 3*C3/4 + C4*exp(2*t**2)/4 - C4/4 + exp(2*t**2)*Integral(t**2*exp(-2*t**2), t))]]

In [29]: sol = _[0]                                                                                                                                                                                         

In [30]: checksysodesol(eqs, sol)                                                                                                                                                                           
Out[30]: (True, [0, 0, 0, 0])

```

## Implicit systems
Support for seemingly non-linear system, if solved for the highest order terms gives us a list of linear systems that can be solved separately was added by this project.
```
In [34]: eqs                                                                                                                                                                                                
Out[34]: 
[Eq(Derivative(f(t), t)**2 - 2*Derivative(f(t), t) + 1, 4),
 Eq(-y*f(t) + Derivative(g(t), t), 0)]

In [35]: dsolve_system(eqs)                                                                                                                                                                                 
Out[35]: 
[[Eq(f(t), C1 + Integral(-1, t)),
  Eq(g(t), C1*t*y + C2*y + t*y*Integral(-1, t) + y*Integral(t, t))],
 [Eq(f(t), C1 + Integral(3, t)),
  Eq(g(t), C1*t*y + C2*y + t*y*Integral(3, t) + y*Integral(-3*t, t))]]

In [36]: sols = _                                                                                                                                                                                           

In [37]: for sol in sols: 
    ...:     print(checksysodesol(eqs, sol) == (True, [0, 0])) 
    ...:                                                                                                                                                                                                    
True
True
```

## Divisible systems
These are the kind of systems that can be solved by dividing into sub-systems(the sub-systems may or may not depend on other sub-system solutions). The below is an example where the system can clearly be solved for `f(t), g(t)` and `h(t), k(t)` separately where the second sub-system for `h(t), k(t)` can be solved by first solving for `h(t)`and then solving for `k(t)` by substituting `h(t)`.
```
In [43]: eqs1                                                                                                                                                                                               
Out[43]: 
[Eq(Derivative(f(t), t), 2*f(t)),
 Eq(Derivative(g(t), t), f(t)),
 Eq(Derivative(h(t), t), h(t)),
 Eq(Derivative(k(t), t), h(t)**4 + k(t))]

In [44]: dsolve_system(eqs1)                                                                                                                                                                                
Out[44]: 
[[Eq(f(t), 2*C1*exp(2*t)),
  Eq(g(t), C1*exp(2*t) + C2),
  Eq(h(t), C3*exp(t)),
  Eq(k(t), (C4 + Integral(C3**4*exp(3*t), t))*exp(t))]]

In [45]: sol = _[0]                                                                                                                                                                                         

In [46]: checksysodesol(eqs1, sol)                                                                                                                                                                          
Out[46]: (True, [0, 0, 0, 0])
```

## Higher order systems
There are a lot of higher order systems that can be now solved by `dsolve_system`. One of the main ones is where a higher order system is reduced to first order system by introducing dummy variables.
```
In [49]: eqs1                                                                                                                                                                                               
Out[49]: 
[Eq(Derivative(f(t), (t, 2)), 2*f(t) + g(t)),
 Eq(Derivative(g(t), (t, 2)), -f(t))]

In [51]: sys_order = _get_func_order(eqs1, [f(t), g(t)])                                                                                                                                                    

In [52]: sys_order                                                                                                                                                                                          
Out[52]: {f(t): 2, g(t): 2}

In [53]: reduced_system = _higher_order_to_first_order(eqs1, sys_order, t)                                                                                                                                  

In [54]: reduced_system                                                                                                                                                                                     
Out[54]: 
([Eq(Derivative(f_1(t), t), 2*f_0(t) + g_0(t)),
  Eq(Derivative(g_1(t), t), -f_0(t)),
  Eq(Derivative(f_0(t), t), f_1(t)),
  Eq(Derivative(g_0(t), t), g_1(t))],
 [f_0(t), f_1(t), g_0(t), g_1(t)])
```
The system in `reduced_system` variable is the one that is being solved for solving the entire system.

There are lots of other types of higher order systems that are reduced to a solvable form(either first order, or a divisible system) and details about the same are given in the description of this [PR](https://github.com/sympy/sympy/pull/19838) and these blog posts: [Week 9](https://mijo2.github.io/Week9/), [Week 10](https://mijo2.github.io/Week10/), [Week 11](https://mijo2.github.io/Week11/), [Week 12](https://mijo2.github.io/Week12/).

# Current Work
Before the end of GSoC'20, there is a target to finish the following PR:
1. [#19998](https://github.com/sympy/sympy/pull/19998): Added simplification strategies for many of the solutions of the systems of ODEs solvers.

# Future Work
There were lots of addtion to the SymPy's ability to solve the systems, but there are things that I wasn't able to complete on time and things that my mentor and me have decided should be done after this project. They are as follows:
1. **Adding the non-linear solver**: The non-linear solver in the [ODE systems roadmap](https://github.com/sympy/sympy/wiki/ODE-Systems-roadmap) wasn't implemented in this project even though I planned to do so.
2. **Reducing systems of ODEs where the minimum order of a dependent variable is greater than 1**: This method would lead to faster solutions due to reduction of unnecessarily large matrices along with the fact that it would help the module solve more number of systems of ODEs. This method wasn't implemented due to lack of time, it wasn't scheduled and the fact that higher order reduction became a very big update then we anticipated it to be.
3. **Improving the structure of the systems solver**: One of the good things that I was able to implement in this project is I combined the 4 linear first order solvers(6 later on) into a single function named `linodesolve`(on the great suggestion of my mentor) wherein, only the matrices and the non-homogeneous terms(if any) have to be passed in the matrix format and the result is given out in matrix format. But, for higher order reduction, I implemented the old structure of classify the system to a type and then solve instead of extending `linodesolve` to take in all the coefficient matrices of all the orders and solve them. I think someone in the future can extend `linodesolve` with matrix operations to get a function where an end user can pass the coefficient matrices obtained from `linear_ode_to_matrix` and just pass it to `linodesolve` to get the vector solution.

# Conclusion
In conclusion, I would say that not only lots of code was removed and replaced by the new solvers, but a new method to solve the systems was implemented, namely the matrix method, which gives us many solvers that are more compact and can handle so many number of types without the restraint that the system needs to be only 2 or 3 equations or only second order in many cases. The best part about implementing these methods was the fact that this may be used by many end-users in so many awesome projects and applications. 

I would like to thank my mentor [Oscar Benjamin](https://github.com/oscarbenjamin) for his valuable suggestions, great solutions to so many types of systems(I have just implemented them) and his teachings when I needed them from day 1 in open source. I would also like to thank the creators for such a great library and the maintainers for keeping it great. I would love to contribute to SymPy in future as well.
