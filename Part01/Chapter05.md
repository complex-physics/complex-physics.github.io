**Measurement based quantum computation**



<!-- toc -->



Reference

1. Jozsa, Richard. "An introduction to measurement based quantum computation." *NATO Science Series, III: Computer and Systems Sciences. Quantum Information Processing-From Theory to Experiment* 199 (2006): 137-158.
2. Browne, Dan, and Hans Briegel. "One‐way Quantum Computation." *Quantum information: From foundations to quantum technology applications* (2016): 449-473.
3. Petros Wallden, “Introduction to Quantum Computing” Lecture 16-18: Measurement-Based Quantum Computing (2018) 
4. [Advanced Quantum Algorithms](https://www.chalmers.se/en/centres/wacqt/graduate%20school/aqa/Documents/FullLectureNotes.pdf)  Lecture by Giulia Ferrini, Anton Frisk Kockum, Laura García-Álvarez, Pontus Vikstål (2020)



# The basic idea of MBQC

In the circuit model of quantum computation, we prepare 

1. (input) initialize the qubits in the input state.
   - Quantum bits (qubits) : represent the data.
2. (computation) Quantum gates on the qubits to manipulate the data.
3. (output) Measurements on the qubits to read out the result.

It seems logically natural to perform operations in this order, which corresponds to our intuition about how classical computations work. While the basic strategy of Measurement based quantum computation:

1. Initialize with a large entangled state.
2. Make single-qubit measurements in suitably chosen bases.

Because of the presence of measurements are done before we reach the output state in MBQC, that computation cannot be reversed. For this reason, Measurement based quantum computation (MBQC) also known as one-way quantum computer). 



## Example

Many of the features of one-way quantum computation can be illustrated in a simple two qubit example. 

### Resource States

Consider the following simple protocol; 

- A qubit is prepared in an unknown state $|\psi\rangle=\alpha|0\rangle+\beta|1\rangle$. 
- A second qubit is prepared in the state $|+\rangle=\frac{1}{\sqrt{2}}(|0\rangle+|1\rangle)$. 

Entangling the total state by apply  controlled-Z gate ($\mathrm{CZ}$) on the two qubits
$$
\begin{aligned}
C Z(|\psi\rangle \otimes|+\rangle) &=[|0\rangle\langle 0|\otimes 1+| 1\rangle\langle 1| \otimes \hat{Z}]\left((\alpha|0\rangle+\beta|1\rangle) \otimes \frac{1}{\sqrt{2}}(|0\rangle+|1\rangle)\right) \\
&=\left[\alpha|0\rangle \otimes \frac{1}{\sqrt{2}}(|0\rangle+|1\rangle)+\beta|1\rangle \otimes \frac{1}{\sqrt{2}}(|0\rangle-|1\rangle)\right] \\
&=\alpha|0\rangle|+\rangle+\beta|1\rangle|-\rangle
\end{aligned}
$$

### Measurements

The next step is to measure the input qubit in some rotated basis.

- Measuring in a rotated basis with angles $(\vartheta, \varphi)$ is like measuring $R_{z}(\varphi+\pi / 2) R_{x}(\vartheta) Z R_{x}(-\vartheta) R_{z}(-\varphi-\pi / 2)$. 
- Here we measure the first qubit with $\vartheta=\pi / 2$ and an arbitrary $\varphi$, i.e., 

we measure the observable
$$
\begin{aligned}
\hat{\sigma}_{\varphi} & \equiv R_{z}(\varphi+\pi / 2) R_{x}(\pi / 2) Z R_{x}(-\pi / 2) R_{z}(-\varphi-\pi / 2) \\
&=\cos \varphi X+\sin \varphi Y \\
&=e^{-i \varphi}|0\rangle\left\langle 1\left|+e^{i \varphi}\right| 1\right\rangle\langle 0|\\
&=\left| \varphi_{+}\right\rangle\left\langle\varphi_{+}|-| \varphi_{-}\right\rangle\left\langle\varphi_{-}\right|
\end{aligned}
$$
where

- the rotated measurement basis $\left|\varphi_{\pm}\right\rangle=1 / \sqrt{2}\left(|0\rangle \pm e^{i \varphi}|1\rangle\right)$. 

conversely $\left|\varphi_{\pm}\right\rangle=1 / \sqrt{2}\left(|0\rangle \pm e^{i \varphi}|1\rangle\right)$ 

- $|0\rangle=\frac{1}{\sqrt{2}}\left(\left|\varphi_{+}\right\rangle+\left|\varphi_{-}\right\rangle\right)$ 
- $|1\rangle=\frac{1}{\sqrt{2}} e^{-i \varphi}\left(\left|\varphi_{+}\right\rangle-\left|\varphi_{-}\right\rangle\right)$

and apply into the state to rewrite 
$$
\begin{aligned}
C Z(|\psi\rangle \otimes|+\rangle)
&=\alpha|0\rangle|+\rangle+\beta|1\rangle|-\rangle\\
&=\frac{\alpha}{\sqrt{2}}\left(\left|\varphi_{+}\right\rangle+\left|\varphi_{-}\right\rangle\right)|+\rangle+\frac{\beta}{\sqrt{2}} e^{-i \varphi}\left(\left|\varphi_{+}\right\rangle-\left|\varphi_{-}\right\rangle\right)|-\rangle \\
&=\left|\varphi_{+}\right\rangle\left(\frac{\alpha}{\sqrt{2}}|+\rangle+\frac{\beta}{\sqrt{2}} e^{-i \varphi}|-\rangle\right)+\left|\varphi_{-}\right\rangle\left(\frac{\alpha}{\sqrt{2}}|+\rangle-\frac{\beta}{\sqrt{2}} e^{-i \varphi}|-\rangle\right)
\end{aligned}
$$
the measurement operator $ \hat{\sigma}_{\varphi} =\left|\varphi_{+}\right\rangle\left\langle\varphi_{+}|-| \varphi_{-}\right\rangle\left\langle\varphi_{-}\right|$ projects the second qubit into:

- $|\psi\rangle_{\text {out }} \propto\left(\alpha|+\rangle+\beta e^{-i \varphi}|-\rangle\right)$ if the outcome is $1$
- $|\psi\rangle_{\text {out }} \propto\left(\alpha|+\rangle-\beta e^{-i \varphi}|-\rangle\right)$ if the outcome is $-1$

The state of the second qubit can then be compactly written as
$$
|\psi\rangle_{\text {out }}=X^m H R_z(-2 \varphi)|\psi\rangle
$$
since
$$
\begin{aligned}
|\psi\rangle_{\text {out }}=& X^m H R_z(-2 \varphi)|\psi\rangle
\\=& X^m H R_z(-2 \varphi)(\alpha|0\rangle+\beta|1\rangle)
\\=& X^m H\left(\alpha e^{i \varphi}|0\rangle+\beta e^{-i \varphi}|1\rangle\right)
\\=& X^m\left(\alpha e^{i \varphi}|+\rangle+\beta e^{-i \varphi}|-\rangle\right)
\end{aligned}
$$
where the $m$ corresponding to

- $|\psi\rangle_{\text {out }} \propto\left(\alpha|+\rangle+\beta e^{-i \varphi}|-\rangle\right)$ if the outcome is $1(m=0)$
- $|\psi\rangle_{\text {out }} \propto\left(\alpha|+\rangle-\beta e^{-i \varphi}|-\rangle\right)$ if the outcome $-1(m=1)$

The extra Pauli operator $X^m$ depends on the outcome of the measurement on qubit 1



## Universal single-qubit operations

For a four qubits system

<img src="https://jptanjing.oss-cn-beijing.aliyuncs.com/jptanjing/QC/image-20220926122306196.png" style="zoom:67%;" />

We can break the pattern of the figure to 3 steps:

1. first consider qubit 1 and qubit 2 alone , 
2. then using qubit 2 as input and qubit 3 as output, 
3. finally, using qubit 3 as input and qubit 4 as output

 The final output state (qubit 4) then becomes
$$
\begin{aligned}
|\psi\rangle_{\text {out }} &=X^{m_{3}} H R_{z}\left(\varphi_{3}\right) X^{m_{2}} H R_{z}\left(\varphi_{2}\right) X^{m_{1}} H R_{z}\left(\varphi_{1}\right)|\psi\rangle \\
&=H Z^{m_{3}} R_{z}\left(\varphi_{3}\right) H Z^{m_{2}} R_{z}\left(\varphi_{2}\right) H Z^{m_{1}} R_{z}\left(\varphi_{1}\right)|\psi\rangle \\
&=H Z^{m_{3}} R_{z}\left(\varphi_{3}\right)\left(H Z^{m_{2}} H\right)\left(H R_{z}\left(\varphi_{2}\right) H\right) Z^{m_{1}} R_{z}\left(\varphi_{1}\right)|\psi\rangle \\
&=H Z^{m_{3}} R_{z}\left(\varphi_{3}\right) X^{m_{2}} R_{x}\left(\varphi_{2}\right) Z^{m_{1}} R_{z}\left(\varphi_{1}\right)|\psi\rangle \\
&=X^{m_{3}} Z^{m_{2}} X^{m_{1}} H R_{z}\left((-1)^{m_{2}} \varphi_{3}\right) R_{x}\left((-1)^{m_{1}} \varphi_{2}\right) R_{z}\left(\varphi_{1}\right)|\psi\rangle
\end{aligned}
$$
where

- in the first step we used that $X^{m} H=H Z^{m}$, 
- in the third that $H Z^{m} H=X^{m}$ 
- and later on that $X R_{z}(\varphi)=R_{z}(-\varphi) X$ 
- and $Z R_{x}(\varphi)=R_{x}(-\varphi) Z$.

Due to Euler's rotation theorem any single-qubit $\mathrm{SU}(2)$ rotation can be decomposed as a product of three rotations
$$
R_{z}(\gamma) R_{x}(\beta) R_{z}(\alpha)
$$
Thus, by repeating the simple two-qubit protocol three times. 

- $\varphi_{1}=\alpha$
- $ \varphi_{2}=(-1)^{m_{1}} \beta $
- $ \varphi_{3}=(-1)^{m_{2}} \gamma$

The above steps lets us implement any single-qubit operation





# Entanglement as a resource for computational power

<img src="https://jptanjing.oss-cn-beijing.aliyuncs.com/jptanjing/QC/image-20220926130757786.png" style="zoom:50%;" />

The entangled states used are called graph states. Given graph
$$
G=(V, E)
$$
where

- vertices $V$ 
- edges $E$

Graph states are obtained by

1. Place at each vertex $V$ a qubit at $|+\rangle$
2. For each edge $E$ apply $CZ$ to entangle the vertices

Resulting state: 
$$
|G\rangle=\prod_{(a, b) \in E} CZ^{(a, b)}|+\rangle^{\otimes V}
$$
comment

- Graph states are highly entangled between all qubits. Entanglement remains after measuring some qubits
- Entanglement is "consumed" during the computation $\Rightarrow$ Entanglement are resource of the computation.

## cluster states

If the graph used is subset of $d$-dimensional lattice the state are also known as cluster states

<img src="https://jptanjing.oss-cn-beijing.aliyuncs.com/jptanjing/QC/image-20220926130114899.png" style="zoom:67%;" />