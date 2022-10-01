**Quantum Approximation Optimization Algorithm (QAOA)** 



In this note, I will first introduce the basic idea of QAOA and some of explicit applications of QAOA. This note is organized in the following way:

-  The basic idea of QAOA
- Applicate into several problem
  1. Max Cut Problem
  2. Traverse Ising model
  3. The binary linear least squares (BLLS) problem
- Development for QAOA

  1. From the QAOA to a Quantum Alternating Operator Ansatz
  2. Relation to the Quantum Adiabatic 

Since QAOA is an approximation algorithm, it does not deliver the 'best' result, but rather the 'good enough' one, characterized by a lower bound of the approximation ratio.



Reference

1. An Introduction to Quantum Optimization Approximation Algorithm. Qingfeng Wang, Tauqir Abdullah 2018
2. Quantum approximate optimization for hard problems in linear algebra. Ajinkya Borle, Vincent E. Elfving and Samuel J. Lomonaco. SciPost Phys. Core 4, 031 (2021) 
3. 



# The basic idea of QAOA

The basic idea of Quantum Approximation Optimization Algorithm (QAOA)

1. The solution to the problem are encoded into the quantum state $\left|\psi\left(\boldsymbol{\beta}_{\text {opt }}, \boldsymbol{\gamma}_{\text {opt }}\right)\right\rangle$ 
2. We first uses a unitary $U(\boldsymbol{\beta}, \boldsymbol{\gamma})$ to prepare a initial quantum state $|\psi(\boldsymbol{\beta}, \gamma)\rangle$. 
3. The goal of the algorithm is to find optimal parameters $\left(\beta_{\text {opt }}, \gamma_{o p t}\right)$ 
   - 







# Max Cut Problem

Given an undirected graph
$$
G=(V, E)
$$
where

- $V$ is the set of vertices
-  $E$ is the set of edges with nonnegative edge weights $w_{i j}=w_{j i}$

Partition the set of vertices of a graph into two categories by setting values

- If vertex $i$ belong to $\bar{S}$, then $s(i)=-1$
- If vertex $i$ belong to $S$, then $s(i)=1$

<img src="https://jptanjing.oss-cn-beijing.aliyuncs.com/jptanjing/QC/image-20220929131855631.png" style="zoom:50%;" />

Therefore

- A cut occurs if two vertices of an edge disagrees, the edge is a cut $s(i) s(j)=-1$. 
- if two vertices belong to the same category, the edge is not a cut $s(i) s(j)=1$



## Cost function

The corresponding Max-Cut Hamiltonian then reads:
$$
\hat{H}_C= \frac{1}{2} \sum_{1 \leq i<j \leq n} w_{i j}\left(1-s_i s_j\right)
$$
The goal is to find a classification that allows the maximum number of cuts to be made
$$
\operatorname{maximize} \frac{1}{2} \sum_{1 \leq i<j \leq n} w_{i j}\left(1-s_i s_j\right)
$$
The eigenstate to this Hamiltonian $\hat{H}_C$ with the highest eigenvalue corresponds to the maximum cut. The formulation $\hat{H}_C$ is also called 'Cost function'
$$
\begin{aligned}
C=&\sum_{\langle ij \rangle} C_{\langle ij \rangle}\\
=&\frac{1}{2} \sum_{1 \leq i<j \leq n} w_{i j}\left(1-s_i s_j\right)
\end{aligned}
$$
Because

1. if two vertices belong to $\bar{S}$, then $s(i)=s(j)=-1$

$$
\begin{aligned}
C_{\langle i j\rangle}=&\frac{1}{2}w_{i j}\left(1-s_i s_j\right)\\
=&\frac{1}{2}w_{i j}\left(1-(-1)(-1)\right)\\
=&0
\end{aligned}
$$

2. if two vertices belong to $S$, then $s(i)=s(j)=1$

$$
\begin{aligned}
C_{\langle i j\rangle}=&\frac{1}{2}w_{i j}\left(1-s_i s_j\right)\\
=&\frac{1}{2}w_{i j}\left(1-1\times 1)\right)\\
=&0
\end{aligned}
$$

3. if two vertices belong to different categories 

$$
\begin{aligned}
C_{\langle i j\rangle}=&\frac{1}{2}w_{i j}\left(1-s_i s_j\right)\\
=&\frac{1}{2}w_{i j}\left(1-(-1)\times 1\right)\\
=& w_{i j}
\end{aligned}
$$

Therefore. the Cost function $C$ is the sum of weights of cutting edge. 

## State

In this section, we will use a example of 4 vertices graph to show how to present states of system by Dirac notation. 

<img src="https://jptanjing.oss-cn-beijing.aliyuncs.com/jptanjing/QC/image-20220929233107156.png" style="zoom:80%;" />

The **ket** denote states of system $|ABCD\rangle$

- If vortex $A$ belong to category $S$,  the place of  $A$ in ket denote 0, that is $|0BCD\rangle$
- If vortex $A$ belong to category $\bar{S}$,  the place of  $A$ in ket denote 1, that is $|1BCD\rangle$

All possible cuts of this 4 vertices graph are

<img src="https://jptanjing.oss-cn-beijing.aliyuncs.com/jptanjing/QC/image-20220930082507995.png" align="center" style="zoom:80%;" />

We can find the Max-Cut state are $|0110\rangle$ and $|1001\rangle$ and max-cut number is 4.



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



# Development 



### Farhi 2014

To be continued
