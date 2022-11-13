To be continued

**Quantum Approximation Optimization Algorithm (QAOA)** 



In this note, I will first introduce the basic idea of QAOA and some of explicit applications of QAOA. This note is organized in the following way:

-  The basic idea of QAOA
-  Applicate into several problem
   1. Max Cut Problem
   2. Traverse Ising model
   3. The binary linear least squares (BLLS) problem
-  Development for QAOA

   1. Relation to the Quantum Adiabatic 
   2. From the QAOA to a Quantum Alternating Operator Ansatz

Since QAOA is an approximation algorithm, it does not deliver the 'best' result, but rather the 'good enough' one, characterized by a lower bound of the approximation ratio.



Reference

1. An Introduction to Quantum Optimization Approximation Algorithm. Qingfeng Wang, Tauqir Abdullah 2018
2. Quantum approximate optimization for hard problems in linear algebra. Ajinkya Borle, Vincent E. Elfving and Samuel J. Lomonaco. SciPost Phys. Core 4, 031 (2021) 
3. Application of the quantum approximate optimization algorithm to combinatorial optimization problems. 
   PONTUS VIKSTÅL, 2020



<!-- toc -->



# The basic idea of QAOA

The basic idea of Quantum Approximation Optimization Algorithm (QAOA)

1. The solution to the problem are encoded into the quantum state $\left|\psi\left(\boldsymbol{\beta}_{\text {opt }}, \boldsymbol{\gamma}_{\text {opt }}\right)\right\rangle$ 
2. We uses a unitary $U(\boldsymbol{\beta}, \boldsymbol{\gamma})$ to prepare a quantum state $|\psi(\boldsymbol{\beta}, \gamma)\rangle$. 
3. The goal of the algorithm is to find optimal parameters $\left(\beta_{\text {opt }}, \gamma_{o p t}\right)$ 
   - This part of the optimization is performed by a classical computer

![](https://jptanjing.oss-cn-beijing.aliyuncs.com/jptanjing/QC/image-20221001095047992.png)

This algorithm is based on two unitaries $U(C, \gamma), U(B, \beta)$ and an integer $p \geq 1$. 

- The two unitaries corresponds to some rotations on our qubits
- The integer $p$ denotes the depth of our circuit.

The first unitary operator depends on an angle $\gamma$ and a Hamiltonian of cost function 
$$
U(C, \gamma)=e^{-i \gamma \hat{H}_C}=\prod_{(i, j) \in E} e^{-i \gamma \hat{H}_{C \langle i j\rangle}}
$$
where

- $\gamma$ lies between 0 and $2 \pi$.
- $\hat{H}_C=\frac{1}{2} \sum_{1 \leq i<j \leq n} w_{i j}\left(1-s_i s_j\right)$ The Hamiltonian of cost function encode the problem

The second unitary operator depends on an angle $\gamma$ and a Hamiltonian $\hat{H}_B$  
$$
\begin{aligned}
U(B, \beta)=& e^{-i \beta B}
\\=& e^{-i \beta \sum_{j=1}^n \sigma_j^x}\\=&\prod_{j=1}^n e^{-i \beta \sigma_j^x}
\end{aligned}
$$
where

- choosing $\hat{H}_B$ to be $\hat{H}_B=\sum_i \hat{\sigma}_i^x$

The total evolution operator become
$$
\begin{aligned}
\hat{U}(\boldsymbol{\beta}, \boldsymbol{\gamma}) &=\prod_{k=1}^pU(\beta_k) U(\gamma_k)
\\&=\prod_{k=1}^p \mathrm{e}^{-\mathrm{i} \beta_k \hat{H}_B} \mathrm{e}^{-\mathrm{i} \gamma_k \hat{H}_C}
\end{aligned}
$$
where 

- for each level $k$ we apply the unitary $U\left(C, \gamma_k\right)$ followed by the unitary $U\left(B, \beta_k\right)$
- total number of level are $p$ 

the final variational "QAOA" state is obtained

$$
\left|\psi_p(\vec{\gamma}, \vec{\beta})\right\rangle =U\left(B, \beta_p\right) U\left(C, \gamma_p\right) \cdots U\left(B, \beta_2\right) U\left(C, \gamma_2\right) U\left(B, \beta_1\right) U\left(C, \gamma_1\right)|+\rangle^{\otimes n}
$$
where

- the state depends on the angles $\vec{\gamma}=\left(\gamma_1, \gamma_2, \ldots, \gamma_p\right)$ and $\vec{\beta}=\left(\beta_1, \beta_2, \ldots, \beta_p\right)$
- The starting state is the superposition of all possible states in the computational basis with equal probability

$$
|+\rangle^{\otimes n} \equiv H^{\otimes n}|0\rangle^{\otimes n}=\frac{1}{\sqrt{2^n}} \sum_{z \in\{0,1\}^n}|z\rangle
$$

The expectation value of $\hat{H}_C$ in this state
$$
F_p(\vec{\gamma}, \vec{\beta}) \equiv\left\langle\psi_p(\vec{\gamma}, \vec{\beta})\left|\hat{H}_C\right| \psi_p(\vec{\gamma}, \vec{\beta})\right\rangle
$$
Let $M_p$ be the maximum of $F_{\max }$ over the angles
$$
F_{\max } =\max _{\vec{\gamma}, \vec{\beta}} F_p(\vec{\gamma}, \vec{\beta})
$$
To maximize the expectation value above, find good angles $\vec{\gamma}=\left(\gamma_1, \gamma_2, \ldots, \gamma_p\right)$ and $\vec{\beta}=\left(\beta_1, \beta_2, \ldots, \beta_p\right)$ by querying a classical optimizer. Several classical optimizers has been implemented.

1. Gradient descent [arXiv:2004.04197](https://arxiv.org/abs/2004.04197)
2. Bayesian optimization  **DOI:** [10.1109/JPROC.2015.2494218](https://doi.org/10.1109/JPROC.2015.2494218)
3. Nelder-mead  https://doi.org/10.1093/comjnl/7.4.308

# Application

1. Max Cut Problem
2. Traverse Ising model
3. The binary linear least squares (BLLS) problem

## Max Cut Problem

​	

Consider cost function $C$ as a operator, then act on the basis state $|z\rangle$ 
$$
C|z\rangle=\sum_{\langle j k\rangle} C_{\langle j k\rangle}(z)|z\rangle=C(z)|z\rangle
$$
where
$$
\begin{aligned}
C_{\langle j k\rangle}|z\rangle=&\frac{1}{2}\left(-s_j \otimes s_k+I\right)|z\rangle
\\=&  \begin{cases}|z\rangle, & \text { if edge }\langle j k\rangle \text { is a cut, that is, } s_j \otimes s_k|z\rangle=-I \\ 0, & \text { if edge }\langle j k\rangle \text { is not a cut, that is, } s_j \otimes s_k|z\rangle=I\end{cases}
\end{aligned}
$$


To be continued



## Traverse Ising model

Traverse filed Ising model (TFIM) describe 1D chain with $n$ interacting spin, the Hamiltonian of the model 
$$
H=\sum_{j=1}^n J_j \sigma_j^z \sigma_{j+1}^z+\sum_{j=1}^n h_j \sigma_j^x
$$
where

- interacting strength $J_j$
- the external magnetic field $h_j$
- $\sigma_j^z$ and $\sigma_j^x$ are Pauli matrixs

To solve traverse filed Ising model with QAOA, we use the Hamiltonian of TFIM as $\hat{H}_C$ 
$$
\hat{H}_C=\hat{H}
$$
Thus
$$
\begin{aligned}
e^{-i \gamma H_C} &=e^{-i \gamma H}\\
&=e^{-i \gamma\left(\sum_{j=1}^n \sigma_j^z \sigma_{j+1}^z+\sum_{j=1}^n \sigma_j^x\right)} \\
&=e^{-i \gamma \sum_{j=1}^n \sigma_j^x} e^{-i \gamma \sum_{j=1}^n \sigma_j^z \sigma_{j+1}^z}
\end{aligned}
$$
Choosing $\hat{H}_B$ to be
$$
\hat{H}_B=\sum_j \hat{\sigma}_j^x
$$
evolution operator become
$$
\begin{aligned}
\hat{U}&=U(\beta) U(\gamma)
\\&=e^{-i \beta \hat{H}_B} e^{-i \gamma \hat{H}_C}
\\&=\left(e^{-i \beta \sum_{j=1}^n \sigma_j^x}\right)\left(e^{-i \gamma \sum_{j=1}^n \sigma_j^x} e^{ -i \gamma \sum_{j=1}^n \sigma_j^z \sigma_{j+1}^z }\right) \\
&=e^{-i(\beta+\gamma) \sum_{j=1}^n \sigma_j^x} e^{-i \gamma \sum_{j=1}^n \sigma_j^z \sigma_{j+1}^z} \\
&=e^{-i \beta^{\prime} \sum_{j=1}^n \sigma_j^x} e^{-i \gamma \sum_{j=1}^n \sigma_j^z \sigma_{j+1}^z}
\end{aligned}
$$
where

- denote $\beta^{\prime}=\beta+\gamma$ as new $\beta$

The expectation value contains two parts.
$$
\begin{aligned}
\langle H\rangle &= \langle\psi |\sum_{j=1}^n \sigma_j^z \sigma_{j+1}^z+\sum_{j=1}^n \sigma_j^x | \psi \rangle \\
&= \langle\psi |\sum_{j=1}^n \sigma_j^z \sigma_{j+1}^z | \psi \rangle+ \langle\psi |\sum_{j=1}^n \sigma_j^z \sigma_{j+1}^z | \psi \rangle \\
&=\left\langle H_1\right\rangle+\left\langle H_2\right\rangle
\end{aligned}
$$

### example: 2-qubit system

The general form of final state of 2-qubit system
$$
|\psi\rangle=a_{00}|00\rangle+a_{01}|01\rangle+a_{10}|10\rangle+a_{11}|11\rangle
$$
where

- The coefficients $\left\{a_{00}, a_{01}, a_{10}, a_{01}\right\}$ are given by the probability distribution from the circuit result. 

The expectation value of first part.
$$
\begin{aligned}
\left\langle\psi\left|H_1\right| \psi\right\rangle=& \langle\psi |\sum_{j=0}^1 \sigma_0^z \sigma_1^z | \psi \rangle \\
=&\left(a _ { 0 0 } \left\langle0 0 \left|+a_{01}\left\langle0 1 \left|+a_{10}\left\langle 10\left|+a_{11}\langle 11|\right)
 \left(\sigma_0^z \sigma_1^z+\sigma_1^z \sigma_0^z\right) 
 \left(a_{00}|00\rangle+a_{01}|01\rangle+a_{10}|10\rangle+a_{11} \mid 11\right\rangle \right) \right.\right.\right.\right.\right.\\
=&\left(a _ { 0 0 } \left\langle0 0 \left|+a_{01}\left\langle0 1 \left|+a_{10}\left\langle 10\left|+a_{11}\langle 11|\right) \sigma_0^z \sigma_1^z\left(a_{00}|00\rangle+a_{01}|01\rangle+a_{10}|10\rangle+a_{11} \mid 11\right)\right\rangle\right.\right.\right.\right.\right.\\
&+\left(a _ { 0 0 } \left\langle0 0 \left|+a_{01}\left\langle0 1 \left|+a_{10}\left\langle 10\left|+a_{11}\langle 11|\right) \sigma_1^z \sigma_0^z\left(a_{00}|00\rangle+a_{01}|01\rangle+a_{10}|10\rangle+a_{11} \mid 11\right)\right\rangle\right.\right.\right.\right.\right.\\
=&\left(a _ { 0 0 } \left\langle0 0 \left|+a_{01}\left\langle0 1 \left|+a_{10}\left\langle 10\left|+a_{11}\langle 11|\right)\left(a_{00}|00\rangle-a_{01}|01\rangle-a_{10}|10\rangle+a_{11} \mid 11\right)\right\rangle\right.\right.\right.\right.\right.\\
&+\left(a _ { 0 0 } \left\langle0 0 \left|+a_{01}\left\langle0 1 \left|+a_{10}\left\langle 10\left|+a_{11}\langle 11|\right)\left(a_{00}|00\rangle-a_{01}|01\rangle-a_{10}|10\rangle+a_{11} \mid 11\right)\right\rangle\right.\right.\right.\right.\right.\\
=& 2\left(a_{00}^2-a_{01}^2-a_{10}^2+a_{01}^2\right)
\end{aligned}
$$
For second part, we apply $H$ gate to the circuit to get the new state
$$
\begin{aligned}
H|\psi\rangle =& H \left(a_{00}|00\rangle+a_{01}|01\rangle+a_{10}|10\rangle+a_{11}|11\rangle\right)
\\= &\left(a_{00}+a_{01}+a_{10}+a_{11}\right)|00\rangle
\\&+\left(a_{00}-a_{01}+a_{10}
-a_{11}\right)|01\rangle
\\& +\left(a_{00}+a_{01}-a_{10}-a_{11}\right)|10\rangle
\\&+\left(a_{00}-a_{01}-a_{10}+a_{11}\right)|11\rangle
\\= & b_{00}|00\rangle+b_{01}|01\rangle+b_{10}|10\rangle+b_{11}|11\rangle
\end{aligned}
$$
where we denote

- $b_{00}=\left(a_{00}+a_{01}+a_{10}+a_{11}\right)$
- $b_{01}=\left(a_{00}-a_{01}+a_{10}-a_{11}\right)$
- $b_{10}=\left(a_{00}+a_{01}-a_{10}-a_{11}\right)$
- $b_{11}=\left(a_{00}-a_{01}-a_{10}+a_{11}\right)$

The $H$ gate also make $\sigma^x \stackrel{H}{\longrightarrow} \sigma^z$, Thus the expectation value of second part.
$$
\begin{aligned}
\left\langle H_2\right\rangle=&\left(b _ { 0 0 } \left\langle0 0 \left|+b_{01}\left\langle0 1 \left|+b_{10}\left\langle 10\left|+b_{11}\langle 11|\right)\left(\sigma_0^z+\sigma_1^z\right)\left(b_{00}|00\rangle+b_{01}|01\rangle+b_{10}|10\rangle+b_{11}|11\rangle\right)\right.\right.\right.\right.\right.\right.\\
=&\left(b _ { 0 0 } \left\langle0 0 \left|+b_{01}\left\langle0 1 \left|+b_{10}\left\langle 10\left|+b_{11}\langle 11|\right)\left(\sigma_0^z\right)\left(b_{00}|00\rangle+b_{01}|01\rangle+b_{10}|10\rangle+b_{11}|11\rangle\right)\right.\right.\right.\right.\right.\right.\\
&+\left(b _ { 0 0 } \left\langle0 0 \left|+b_{01}\left\langle0 1 \left|+b_{10}\left\langle 10\left|+b_{11}\langle 11|\right)\left(\sigma_1^z\right)\left(b_{00}|00\rangle+b_{01}|01\rangle+b_{10}|10\rangle+b_{11}|11\rangle\right)\right.\right.\right.\right.\right.\right.\\
=& b_{00}^2+b_{01}^2-b_{10}^2-b_{11}^2+b_{00}^2-b_{01}^2+b_{10}^2-b_{11}^2 \\
=& 2 b_{00}^2-2 b_{11}^2 \\
=& 2\left(a_{00} a_{01}+a_{00} a_{10}+a_{11} a_{01}+a_{11} a_{10}\right)
\end{aligned}
$$
In last step, we transform notation from $b$ to $a$. We can find this result is the same with 
$$
\begin{aligned}
\left\langle H_2\right\rangle &=\left(a _ { 0 0 } \left\langle0 0 \left|+a_{01}\left\langle0 1 \left|+a_{10}\left\langle 10\left|+a_{11}\langle 11|\right)\left(\sigma_0^x+\sigma_1^x\right)\left(a_{00}|00\rangle+a_{01}|01\rangle+a_{10}|10\rangle+a_{11} \mid 11\right)\right\rangle\right.\right.\right.\right.\right.\\
&=2\left(a_{00} a_{01}+a_{00} a_{10}+a_{11} a_{01}+a_{11} a_{10}\right)
\end{aligned}
$$
Which we calculate directly without change of basis.

## BLLS problem 

> Reference
>
> - Quantum approximate optimization for hard problems in linear algebra. Ajinkya Borle, Vincent E. Elfving and Samuel J. Lomonaco. SciPost Phys. Core 4, 031 (2021) 

The binary linear least squares (BLLS) problem



# Development 

## History

1. QAOA was first proposed by Farhi etal in 2014. ([arXiv:1411.4028](https://arxiv.org/abs/1411.4028)) 
   - they applied QAOA on MaxCut problem
   - they showed the complexity of problem depends on $p$ rather than $n$. 
   - they showed the relationship between QAOA and quantum adiabatic algorithm (QAA)
2. QAOA can exhibit a form of Quantum Supremacy. 2016 ([arXiv:1602.07674](https://arxiv.org/abs/1602.07674))
3. 



To be continued
