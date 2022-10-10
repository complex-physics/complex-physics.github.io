To be continued

HHL



![](https://jptanjing.oss-cn-beijing.aliyuncs.com/jptanjing/QC/image-20221002143334632.png)

Reference

1. Lloyd, Seth. "Quantum algorithm for solving linear systems of equations." APS March Meeting Abstracts. Vol. 2010. 2010. [[arXiv:0811.3171](https://arxiv.org/abs/0811.3171)]
2. Morrell Jr, Hector Jose, and Hiu Yung Wong. "Step-by-Step HHL Algorithm Walkthrough to Enhance the Understanding of Critical Quantum Computing Concepts." [arXiv:2108.09004 (2021)](https://arxiv.org/abs/2108.09004).
3. Learn Quantum Computation using Qiskit. [online book](https://qiskit.org/textbook/ch-applications/hhl_tutorial.html)
4. Duan, Bojia, et al. "A survey on HHL algorithm: From theory to application in quantum machine learning." Physics Letters A 384.24 (2020): 126595.



<!-- toc -->

# Basic idea of HHL

## Question

The goal of HHL is solve the linear equation
$$
A \vec{x}=\vec{b}
$$
where 

- matrix $A$ and a vector $\vec{b}$ are given
- find $\vec{x} $ satisfying this linear equation

we can convert the equation into 
$$
\vec{x}=A^{-1} \vec{b}
$$

### example

the linear equation
$$
\begin{aligned}
A \vec{x}&=\vec{b}\\
\left(\begin{array}{cc}
1 & -\frac{1}{3} \\
-\frac{1}{3} & 1
\end{array}\right) \left(\begin{array}{l}
x_0 \\
x_1
\end{array}\right) &= \left(\begin{array}{l}
0 \\
1
\end{array}\right)
\end{aligned}
$$
find $\vec{x}=\left(\begin{array}{c}
\frac{3}{8} \\
\frac{9}{8}
\end{array}\right)$ satisfying this linear equation
$$
\begin{aligned}
A \vec{x}&=\vec{b}\\
\left(\begin{array}{cc}
1 & -\frac{1}{3} \\
-\frac{1}{3} & 1
\end{array}\right) 
\left(\begin{array}{l}
\frac{3}{8} \\
\frac{9}{8}
\end{array}\right) 
&= \left(\begin{array}{l}
0 \\
1
\end{array}\right)
\end{aligned}
$$


## Mathematical Preliminaries

Matrix $A$ should be Hermitian. If not, the $A$ can be converted to Hermitian by
$$
\left(\begin{array}{ll}
0 & A \\
A^{\dagger} & 0
\end{array}\right)
$$
Since matrix $A$ is Hermitian, it has a spectral decomposition
$$
A=\sum_{j=0}^{N-1} \lambda_j\left|u_j\right\rangle\left\langle u_j\right|
$$
where

-  $\lambda_j$ is $j$-th eigenvalue of matrix $A$ 
-  $\left|u_j\right\rangle$ is $j$-th eigenvector of matrix $A$ 

the inverse of $A$ can be written as
$$
A^{-1}=\sum_{j=0}^{N-1} \lambda_j^{-1}\left|u_j\right\rangle\left\langle u_j\right|
$$
Since $A$ is invertible and Hermitian, it must have an orthogonal basis of eigenvectors, and thus we can write $b$ in the eigenbasis of $A$ as
$$
|b\rangle=\sum_{j=0}^{N-1} b_j\left|u_j\right\rangle, \quad b_j \in \mathbb{C}
$$
The eigenbase of $A$ must be orthogonal eigenvectors $\left\langle u_i \mid u_j\right\rangle=\delta_{i j}$ since $A$ is invertible and Hermitian, so we can write $b$ in the eigenbasis of $A$ as
$$
|b\rangle=\sum_{j=0}^{N-1} b_j\left|u_j\right\rangle
$$
The solution of the linear equation also can be written in the eigenbasis
$$
|x\rangle=A^{-1}|b\rangle=\sum_{j=0}^{N-1} \lambda_j^{-1} b_j\left|u_j\right\rangle
$$

## Steps of the HHL algorithm

Overview of the HHL algorithm flow

1. Initialize the vector $\vec{b}$ to be quantum state $|b\rangle$
2. Estimate the eigenvalues of matrix $A$ 
   - Apply Quantum Phase Estimation (QPE) to get the value of
3. Ancilla bit Rotation:
   - 
4. Inverse Quantum Phase Estimation (IQPE)
5. Measurement
   - Measure the auxiliary qubit in the computational basis.

![](https://jptanjing.oss-cn-beijing.aliyuncs.com/jptanjing/QC/image-20221002143334632.png)

### State Preparation

There are total $n_b+n+1$ qubits and they are initialized as
$$
\left|\Psi_0\right\rangle=|0 \cdots 0\rangle_b|0 \cdots 0\rangle_c|0\rangle_a=|0\rangle^{\otimes n_b}|0\rangle^{\otimes n}|0\rangle
$$
where

-  $n_b$ qubits  state $|0 \cdots 0\rangle_b$ in the b-register 
-  $n$ qubits (clock qubits) state $|0 \cdots 0\rangle_c$ in the c-register
-  state $|0\rangle_a$ of ancilla qubit

where registers of quantum circuit are

- b-register
  - rotated to have the amplitudes correspond to the coefficients of $\vec{b}$. That is
- c-register (clock qubits) 
  - related to the time (clock) in the controlled rotation in the QPE
  - c-register stores the values of the phase of the eigenvalues of the $A$ matrix after the QPE. 
  - There are $n$ qubits (clock qubits) in the c-register.
- ancilla qubit
  - 



 $|0 \cdots 0\rangle_b$ in the b-register needs to be rotated to have the amplitudes correspond to the coefficients of $\vec{b}$. That is
$$
\begin{aligned}
\vec{b}&=\left(\begin{array}{c}
\beta_0 \\
\beta_1 \\
\vdots \\
\beta_{N_b-1}
\end{array}\right) 
\\ \Updownarrow 
\\|b\rangle &=\beta_0|0\rangle+\beta_1|1\rangle+\cdots+\beta_{N_b-1}\left|N_b-1\right\rangle
\end{aligned}
$$
After state preparation
$$
\left|\Psi_1\right\rangle=|b\rangle_b|0 \cdots 0\rangle_c|0\rangle_a
$$


### Quantum Phase Estimation

![](https://jptanjing.oss-cn-beijing.aliyuncs.com/jptanjing/QC/image-20221002143334632.png)

Quantum Phase Estimation (QPE) has three components

1. the superposition of the clock qubits through Hadamard gates, 
2. controlled rotation
3. Inverse Quantum Fourier Transform (IQFT). The goal of QPE is to estimate the phase of the eigenvalues of the unitary rotation matrix, $U=e^{i A t}$, in the controlled gate, $C-U$, (Fig. 1) used in the QPE. Again, this gate encodes the matrix $A$

#### Hadamard gates

Apply the Hadamard gates to the clock qubits to create a superposition of the clock qubits
$$
\begin{aligned}
\left|\Psi_2\right\rangle &=I^{\otimes n_b} \otimes H^{\otimes n} \otimes I\left|\Psi_1\right\rangle \\
&=|b\rangle \frac{1}{2^{\frac{n}{2}}}(|0\rangle+|1\rangle)^{\otimes n}|0\rangle
\end{aligned}
$$

#### controlled rotation







applying $A$ to its eigenvector $\left|u_j\right\rangle$,
$$
\begin{aligned}
A\left|u_j\right\rangle &=\sum_{i=0}^{2^{n_b}-1} \lambda_i\left|u_i\right\rangle\left\langle u_i|| u_j\right\rangle \\
&=\sum_{i=0}^{2^{n_b-1}} \lambda_i\left|u_i\right\rangle \delta_{i j} \\
&=\lambda_j\left|u_j\right\rangle
\end{aligned}
$$




### Rotation





## Example: 4-qubit

> Reference: Learn Quantum Computation using Qiskit. [online book](https://qiskit.org/textbook/ch-applications/hhl_tutorial.html)



To solve linear equation
$$
A \vec{x}=\vec{b}
$$
where 

- matrix $A$ and a vector $\vec{b}$ are given
- find $\vec{x} $ satisfying this linear equation





To be continued



