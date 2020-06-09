---
layout: post
title: GSoC'20 - Community Bonding Period
---

I was overwhelmed with joy on May 4 when I saw my name in the list of selected students in the GSoC list of selected students
for SymPy. I went through the process and made my first blog post which detailed my journey with SymPy till I got selected.
In the month of May, I worked for roughly three weeks since my college declared open book exams for final year students and 
given these were my final exams, I took a break of two weeks starting from May 24. I informed my mentor about this and that I
would work harder when my exams end.

The below are the list of tasks that I completed for the Community Bonding Period:

1. Made my first blog post with jekyll.
2. Worked on the non-constant coefficient homogeneous solver with the condition that the coefficient matrix of the system of 
   ODEs is commutative with its anti-derivative. The PR regarding this was merged in this period. Hence, two of the nine tasks
   that are to be completed for this project have been done successfully.
   
   Now the library can handle lots of systems of ODEs that was not possible before, along with the fact that lots of old code 
   and solvers were removed with this PR. The solver is named ```_linear_neq_order1_type3```
3. Created a good first issue wherein some of the test cases from an old PR needed to be added in the master and ensured that 
   the new contributor gets a hands on experience with contributing to the library.
4. Created and almost completed the PR for adding constant coefficient homogeneous solver that can efficiently handle 
