---
title: "Useful matrix decompositions"
teaching: 10
exercises: 0
questions:
- "How can matrix decompositions help with repeated solutions?"
- "What is the LU decomposition?"
- "What is the Cholesky decomposition?"
- "What is the QR decomposition?"

objectives:
- "Explain the main useful matrix decompositions"
---

Matrix factorisations play a key role in the solution of problems of the type $A x = b$. 
Often (e.g. ODE solvers), you have a fixed matrix $A$ that must be solved with many 
different $b$ vectors. A matrix factorisation is effectivly a pre-processing step that 
allows you to partition $A$ into multiple factors (e.g. $A = LU$ in the case of LU 
decomposition), so that the actual solve is as quick as possible.

## LU decomposition

The LU decomposition is closely related to gaussian elimination. It takes the original 
equation to be solved $A x = b$ and splits it up into two separate equations involving a 
unit lower triangular matrix $L$, and the row echelon matrix $U$:

\begin{align}
L y &= b \\
U x &= y
\end{align}


where $A = LU$. The $L$ matrix is a *unit* lower triangular matrix and thus has ones on 
the diagonal, whereas $U$ is in row echelon form with pivot values in the leading 
coefficients of each row.

\begin{equation}
A = \left(\begin{matrix}
1 & 0 & 0 & 0 \\ 
* & 1 & 0 & 0 \\
* & * & 1 & 0 \\
* & * & * & 1 
\end{matrix}\right)
\left(\begin{matrix}
p_1 & * & * & * \\ 
0 & p_2 & * & * \\
0 & 0 & p_3 & * \\
0 & 0 & 0 & p_4 
\end{matrix}\right)
\end{equation}

Thus, we have converted our original problem of solving $A x = b$ into two separate 
solves, first solving the equation $L y = b$, and then using the result $y$ to solve $U 
x = y$. 

\begin{align}
\left(\begin{matrix}
1 & 0 & 0 & 0 \\ 
* & 1 & 0 & 0 \\
* & * & 1 & 0 \\
* & * & * & 1 
\end{matrix}\right)
\left(\begin{matrix}
y_1 \\ 
y_2 \\
y_3 \\
y_4 
\end{matrix}\right)
&= \left(\begin{matrix}
b_1 \\ 
b_2 \\
b_3 \\
b_4 
\end{matrix}\right)
\\

left(\begin{matrix}
p_1 & * & * & * \\ 
0 & p_2 & * & * \\
0 & 0 & p_3 & * \\
0 & 0 & 0 & p_4 
\end{matrix}\right)
\left(\begin{matrix}
x_1 \\ 
x_2 \\
x_3 \\
x_4 
\end{matrix}\right)
&= \left(\begin{matrix}
y_1 \\ 
y_2 \\
y_3 \\
y_4 
\end{matrix}\right)
\end{align}


However, each of those solves is very cheap to compute, in this case for the 4x4 matrix 
shown above the solution of $L y = b$ only needs 6 multiplication and 6 additions, 
whereas $U x = y$ requires 4 divisions, 6 multiplications and 6 additions, leading to a 
total of 28 arithmetic operations, much fewer in comparison with the 62 operations 
required to solve the original equation $A x = b$.


## LU factorisation without pivoting

A relativly simple LU algorithm can be described if we assume that no pivoting is 
required during a gaussian elimination. In this case, the gaussian elimination process 
is a sequence of $p$ linear operations $E_1, E_2, ..., E_p$, with each operation $E_i$ 
being a row replacement that adds a multiple of one row to another below it (i.e. $E_i$ 
is lower triangular). The final matrix after applying the sequence of row reductions is 
$U$ in row echelon form, that is:

$$
E_p \cdots E_2 E_1 A = U
$$

Since we have $A = LU$, we can show that the sequence of operations $E_1, E_2, ..., E_p$ 
is also the sequence that reduces the matrix $L$ to an identity matrix:

$$
A = (E_p \cdots E_2 E_1)^{-1} U = LU,
$$

therefore, 

$$
L = (E_p \cdots E_2 E_1)^{-1},
$$

and,

$$
(E_p \cdots E_2 E_1) L = (E_p \cdots E_2 E_1) (E_p \cdots E_2 E_1)^{-1} = I
$$


This implies how we can build up the matrix $L$. We choose values for $L$ such that the 
series of row operations $E_1, E_2, ..., E_p$ convert the matrix $L$ to the identity 
matrix. Since each $E_i$ is lower triangular, we know that both $(E_p \cdots E_2 E_1)$ 
and $(E_p \cdots E_2 E_1)^{-1}$ are also lower triangular.

For example, consider the following matrix

$$
A = left(\begin{matrix}
3 & 2 & 1 & -3 \\ 
-6 & -2 & 1 & 5 \\
3 & -4 & -7 & 2 \\
-9 & -6 & -1 & 15 
\end{matrix}\right)
$$

After three row reductions, $R_2 += 2 R_1$, $R_3 += -1 R_1$, and $R_3 += 3 R_1$, we have 
the following result:

$$
E_1 E_2 E_3 U = left(\begin{matrix}
3 & 2 & 1 & -3 \\ 
0 & 2 & * & * \\
0 & -6 & * & * \\
0 & 0 & * & * 
\end{matrix}\right)
$$

To build the 1st column of $L$, we simply divide the 1st column of $A$ by the pivot 
value 3, giving

$$
L = left(\begin{matrix}
1  & 0 & 0 & 0 \\ 
-2 & 1 & 0 & 0 \\
1  & * & 1 & 0 \\
-3 & * & * & 1 
\end{matrix}\right)
$$

For the next column we do the same, using the new pivot value $A_{2,2} = 2$ in row 2 to 
reduce $A_{3,2}$ and $A_{4,2}$ to zero, and then dividing the column vector under the 
pivot $(-6, 0)^T$ by the pivot value 2 to obtain the next column of $L$.

Repeating this process for all the columns in $A$, we obtain the final factorisation. 
You can verify for yourself that repeating the same row operations we did to form $U$ to 
the matrix $L$ reduces it to the identity matrix.

$$
L = left(\begin{matrix}
1 & 0 & 0 & 0 \\ 
-2 & 1 & 0 & 0 \\
1 & -3 & 1 & 0 \\
-3 & 0 & 2 & 1 
\end{matrix}\right)
$$

$$
U = left(\begin{matrix}
3 & 2 & 1 & -3 \\ 
0 & 2 & 3 & -1 \\
0 & 0 & 1 & 2 \\
0 & 0 & 0 & 2 
\end{matrix}\right)
$$

## Pivoting

Of course, for any practial LU factorisation we need to consider pivoting. Any matrix 
$A$ can be factorised into $PLU$, where $P$ is a permutation matrix, and $L$ and $U$ are 
defined as before. During the gaussian elimination steps we store an array of row 
indices $p_i$ indicating that row $i$ is interchanged with row $p_i$, and the resultant 
array of $p_i$ can be used to build the permutation matrix $P$ (It would be wasteful to 
store the entire martix $P$ so the array $p_i$ is stored instead). 

Thus, the LU algorithm proceeds as follows:

1. Begin with the left-most column $i=0$, find an appropriate pivot (e.g. maximum entry 
   in the colum) and designate this row as the pivot row. Interchange this row with row 
   $i$, and store the pivot row index as $p_i$. Use row replacements to create zeros 
   below the pivot. Create the corresponding column for $L$ by dividing by the pivot 
   value.
2. Continue along to the next column $i$, again choosing a pivot row $p_i$, 
   interchanging it with row $i$ and creating zeros below the pivot, creating the new 
   column in $L$, and making sure to record which pivot row has been chosen for each 
   column. Repeat this step for all the columns of the matrix.
3. Once the last column has been done, $U$ should be in row echlon form and $L$ should 
   be a unit lower triangular matrix. The array $p_i$ implicitly defines the permutation 
   matrix $P$

In practice, most library implementation store $L$ and $U$ in the same matrix since they 
are lower and upper triangular respectivly.

## Problems

1. Take your gaussian elimination code that you wrote in the previous lesson and use it 
   to write an LU decomposition function that takes in a martix $A$, and returns $L$, 
   $U$ and the array $p_i$. You can check your answer using 
   [`scipy.linalg.lu_factor`](https://docs.scipy.org/doc/scipy/reference/generated/scipy.linalg.lu_factor.html).


## QR decomposition

explain

### Gram-Schmidt Process

### Householder reflections

### Givens Rotations

## Cholesky decomposition

explain

## Problems

Use scipy to solve problems using LU (solve Ax = b using lu), Cholesky (Generating a 
sampled Gaussian field), + QR decomp (least squares problem)


