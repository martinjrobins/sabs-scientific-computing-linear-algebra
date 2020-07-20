---
title: "Solution of systems of linear equations"
teaching: 10
exercises: 0
questions:
- "What is the relationship between matrices and systems of linear equations?"
- "What is a singular matrix and when does it occur?"
- "What is Gaussian Elimination and why is it useful?"

objectives:
- "Understand the main useful concepts for the solution of systems of linear equations"
- "Understand singular matrices and the rank of a matrix"
- "Understand and be able to implement Gaussian Elimination"
---


## Simultaneous equations

Consider 2 simultaneous equations:
\begin{eqnarray}
a_1x+b_1y &=& c_1, \quad (1)\\
a_2x+b_2y &=& c_2, \quad (2)
\end{eqnarray}

where the values $\;x\;$ and $\;y\;$ are to be found, and $\;a_1, \;b_1, \;a_2, \;b_2, \;c_1\;$ and $\;c_2\;$ are given constants.

\begin{eqnarray}
 (1) \times b_2:~~~~~~~~~~~~~~~ b_2a_1x+b_2b_1y &=& b_2c_1, \quad (3)\\
  (2) \times b_1:~~~~~~~~~~~~~~~ b_1a_2x+b_1b_2y &=& b_1c_2, \quad (4)\\
  (3) - (4):~~~~~~~~~~~~~~~ b_2a_1x-b_1a_2x &=& b_2c_1-b_1c_2.
\end{eqnarray}

## The Matrix

Matrices are a structure that allow us to more easily manipulate linear systems. 

Consider the original system

\begin{align}
a_1x+b_1y &= c_1, \\
a_2x+b_2y &= c_2. 
\end{align}

We rewrite this, in the form of a matrix as:

\begin{equation}
\left(\begin{matrix}a_1&b_1\\ a_2&b_2\end{matrix}\right)
\left(\begin{matrix}x\\y\end{matrix}\right)
=\left(\begin{matrix}c_1\\ c_2 \end{matrix}\right).
\label{eq:mat}
\end{equation}
 
Think about how this form relates to the original linear system. 

### The determinant

If

$A = \left(\begin{matrix} p & q \\ r & s\end{matrix}\right)$

then the **determinant** of A is:

$|A| = ps-qr$

The inverse of $A$ can be found using the determinant:

$A^{-1} = \frac{1}{ps-qr} \left(\begin{matrix} s & -q \\ -r & 
p\end{matrix}\right)$.

Calculating the inverse of a matrix using its determinant can be very costly for larger 
matrices, therefore other algorithms are used (e.g. Gaussian Elimination, which is 
introduced below)

If $|A| = 0$, A is said to be **singular** (have no inverse).

## Geometry of linear equations

Consider the following system of equations

\begin{align}
 x + -2y &= -1, \\
-x +  3y &=  3. 
\end{align}

That is,

$A = \left(\begin{matrix} 1 & -2 \\ -1 & 3\end{matrix}\right)$

Plotting these two linear equations on a graph shows graphically the solution to this 
equation given by

\left(\begin{matrix} x \\ y \end{matrix}\right) = A^{-1} \left(\begin{matrix} -1 \\ 3 
\end{matrix}\right) = \left(\begin{matrix} -1 \\ 3 \end{matrix}\right)

![single solution](../fig/01-sim1.pdf)

Now lets consider two different system of equations represented by the matrix

$A = \left(\begin{matrix} 1 & -2 \\ -1 & 2\end{matrix}\right)$

\begin{align}
 x + -2y &= -1, \\
-x +  2y &=  3. 
\end{align}

and 

\begin{align}
 x + -2y &= -1, \\
-x +  2y &=  1. 
\end{align}

![(left) infinite solutions, (right) no solutions](../fig/01-sim2.pdf)

The first gives the plot on the left, while the second, which has a different vector of 
constants on the RHS, gives the plot on the right. You can see that depending on the 
constants, the system of equations can have an infinite number of solutions, or no 
solutions at all. 

The matrix $A$ in this case is singular, and therefore does not have an inverse. Looking 
at the equations again, you can see that the two rows of the matrix $A$ are multiples of 
the other, and thus there is only *one* independent row. That is, the *rank* of the 
matrix is one.

## Singular matrices

The *rank* of an $\;n\,\times\,n\;$ matrix $\;A\;$ is the number of linearly independent rows in $\;A\;$ (rows not combinations of other rows).

When $\;\mbox{rank}(A) < n\;$ then

- The matrix is said to be 'rank deficient'
- The system $\;A\textbf{x} = \textbf{b}\;$ has *fewer* equations than unknowns
- The matrix is said to be singular
- The matrix is said to be underdetermined
- $A\;$ has no inverse
- The determinant of $\;A\;$ is 0
- The equation $\;A\textbf{u} = \textbf{0}\;$ has non-trivial solutions ($\textbf{u} \neq \textbf{0}$)

## Problems

1. Solve to find a combination of the columns that equals b: Triangular system
u − v − w = b1 
v + w = b2 
w = b3.

2. Describe the intersection of the three planes u+v+w+z = 6 and u+w+z = 4 and u+w = 2 
   (all in four-dimensional space). Is it a line or a point or a fourth equation that 
   leaves us with no solution. an empty set? What is the intersection if the fourth 
   plane u = −1 is included? Find a fourth equation that leaves us with no solution.

3. Sketch these three lines and decide if the equations are solvable: 3 by 2 system
x + 2y = 2 x − y = 2 y = 1.
What happens if all right-hand sides are zero? Is there any nonzero choice of right- hand sides that allows the three lines to intersect at the same point?

## Gaussian Elimination

Consider the problem: Find $x, y,z$ such that 

\begin{align}
eq1: &  2x & + & y &  -&  z & = & 3 \\
eq2: & x & &  &+ &5z & = & 6 \\
eq3: & -x &+& 3y& -& 2z & = &  3
\end{align}

\textbf{Gaussian elimination -- step 1} reduce the above system of equations so that the unknown $x$ is removed from the last two equations:

\begin{align}
eq1: & 2x & +& y& -& z & =  &3 \\
eq2 ~\rightarrow eq1  - 2 \times eq2: & & &  y & - &11 z & =& -9 \\
eq3~ \rightarrow ~ eq1 + 2 \times eq3: & & &  7y &- &5z& = & 9 
\end{align}

\textbf{Gaussian elimination -- step 2} remove the unknown $y$ from the last equation:

\begin{align}
eq1: & 2x + y - z & =  3 \\
eq2: & ~~~ y - 11 z & = -9 \\
eq3 ~ \rightarrow ~ 7 \times eq2 - eq3: & ~~~ -72  z = -72 
\end{align}

\begin{align}
eq1: & 2x + y - z & =  3 \\
eq2: & ~~~ y - 11 z & = -9 \\
eq3:  &  ~~~ -72  z & = -72 
\end{align} 

This system is said to be {\bf upper triangular}.

\textbf{Gaussian elimination -- step 3} We can now use back substitution to obtain $x,y,z$.  In this case 

$$ z = 1,$$
$$ eq2: ~~ y - 11 = -9 ~~ \implies ~~ y = 2,$$
$$ eq1: ~~ 2x +2 -1 = 3 , ~~ \implies ~~ x = 1.$$ 

### Another rule for Gaussian elimination

Consider the following system 

\begin{align}
eq1: &  x & + & y & + & z & =  & a \\
eq2: & 2x & + & 2y & + &  5z &  = & b \\
eq3: & 4x & +&  6y  & + & 8z &  = &  c 
\end{align} 

This firstly reduces to 

\begin{align}
eq1: & x  & + & y & + & z & =  & a \\
eq2:& & & & &  3z &   = & b' \\
eq3: & & & 2y &  + & 4z &  = & c' 
\end{align}

This can easily be switched into upper triangular form by switching rows two and three.

### Singular matrices in Gaussian Elimination

\begin{align}
eq1: &  x & + & y & + & z & =  & a \\
eq2: & 2x & + & 2y & + &  5z &  = & b \\
eq3: & 4x & +&  4y  & + & 8z &  = &  c 
\end{align} 

This firstly reduces to 

\begin{align}
eq1: & x  & + & y & + & z & =  & a \\
eq2:& & & & &  3z &   = & b' \\
eq3:& & & & &  4z &  = & c' \end{align}

We cannot solve this by switching rows in this case, which means that the matrix is 
singular and has no inverse

### Gaussian Elimination Rules

1. Operate on LHS and RHS (or RHSs) at the same time
2. Replace row with a sum/combination of rows
3. Switch rows to avoid zeros on the diagonal (*pivoting*)
4. If (3) does not work, zeros on the diagonal (*pivots*) indicate a singular matrix 

**Computational cost**: If the number of equations $n$ is large, then a number of 
operations for gaussian elimination is $\mathcal{O}(n^3)$.

## Problems

1. Code a Python function that takes an 2D numpy array representing a matrix $A$, and a 
   1D array representing a RHS $b$, and returns the solution of the linear equation $Ax 
   = b$. If you wish you can assume that the matrix has an inverse. Try it out on a few 
   test matrices and check your answer using 
   [`scipy.linalg.solve`](https://docs.scipy.org/doc/scipy-0.18.1/reference/generated/scipy.linalg.solve.html).

## Textbooks


Linear algebra by Ward Cheney
Linear algebra and its applications by David C. Lay.

Strang, G. (2016). Introduction to linear algebra (Fifth ed.). Wellesley.

Linear algebra and its applications by Gilbert Strang

lots of supplimentary material for this last two via MIT course page here:
https://github.com/mitmath/1806/blob/master/summaries.md


- LA from an ODE perspective
Kapitula, T. (2015). Ordinary Differential Equations and Linear Algebra. Society for 
Industrial and Applied Mathematics.

## Software

scipy.optimise https://docs.scipy.org/doc/scipy/reference/optimize.html
PINTS: https://pints.readthedocs.io/en/latest/optimisers/index.html 
https://pints-team.github.io/pints-methods-overview/
NLOpt: https://nlopt.readthedocs.io/en/latest/NLopt_Algorithms/
PaGMO: https://esa.github.io/pagmo/

{% include links.md %}
