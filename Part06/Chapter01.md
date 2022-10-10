To be continued

Max Cut Problem



<!-- toc -->



Reference

Given an undirected graph
$$
G=(V, E)
$$
where

- $V$ is the set of vertices
- $E$ is the set of edges with nonnegative edge weights $w_{i j}=w_{j i}$

Partition the set of vertices of a graph into two categories by setting values

- If vertex $i$ belong to $\bar{S}$, then $s(i)=-1$
- If vertex $i$ belong to $S$, then $s(i)=1$

<img src="https://jptanjing.oss-cn-beijing.aliyuncs.com/jptanjing/QC/image-20220929131855631.png" style="zoom:50%;" />

Therefore

- A cut occurs if two vertices of an edge disagrees, the edge is a cut $s(i) s(j)=-1$. 
- if two vertices belong to the same category, the edge is not a cut $s(i) s(j)=1$



### Cost function

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

### State

In this section, we will use a example of 4 vertices graph to show how to present states of system by Dirac notation. 

<img src="https://jptanjing.oss-cn-beijing.aliyuncs.com/jptanjing/QC/image-20220929233107156.png" style="zoom:80%;" />

The **ket** denote states of system $|ABCD\rangle$

- If vortex $A$ belong to category $S$,  the place of  $A$ in ket denote 0, that is $|0BCD\rangle$
- If vortex $A$ belong to category $\bar{S}$,  the place of  $A$ in ket denote 1, that is $|1BCD\rangle$

All possible cuts of this 4 vertices graph are

<img src="https://jptanjing.oss-cn-beijing.aliyuncs.com/jptanjing/QC/image-20220930082507995.png" align="center" style="zoom:80%;" />

We can find the Max-Cut state are $|0110\rangle$ and $|1001\rangle$ and max-cut number is 4.



