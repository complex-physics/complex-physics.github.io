

# Quantum Circuit Model

Quantum circuit model is a quantum analogy of the classical circuit. A computation process in classical circuit requires a system that can represent data(input), a way to perform manipulation of that data(computation), and a method for reading out (output).

- (input) initialize the qubits in the input state.
  - Quantum bits (qubits) : represent the data.
- (computation) Quantum gates on the qubits to manipulate the data.
- (output) Measurements on the qubits to read out the result.

The classical circuits consist of elementary logical operations. (NOT, AND, OR, NAND, etc.) Quantum circuits are also composed of elementary logic operations, which are quantum gates.





## Reference

1. Chapter 1 in 《Advanced Quantum Algorithms》by Giulia Ferrini, Anton Frisk Kockum, Laura García-Álvarez, Pontus Vikstål
2. Quantum Algorithm Implementations for Beginners [arXiv:1804.03719](https://arxiv.org/abs/1804.03719) Los Alamos National Laboratory
3. 《Gates, States, and Circuits》 Notes on the circuit model of quantum computation (2021) Gavin E. Crooks
4. 《Differentiable Quantum Circuits》 2019
5. Michael A Nielsen and Isaac Chuang.《Quantum computation and quantum information》, 2002





<!-- toc -->





# Qubit

The quantum bit (qubit) is a fundamental unit of quantum information. It can be physically realized with a two-level quantum-mechanical system.

A qubit can be described by a linear combination of two orthonormal basis states $|0\rangle$ and $|1\rangle$

$$
\begin{aligned}
|\phi\rangle=\alpha|0\rangle+\beta|1\rangle .
\end{aligned}
$$

These basis states $|0\rangle$ and $|1\rangle$  are usually denoted as 
$$
|0\rangle=\left(\begin{array}{l}
1 \\
0
\end{array}\right), \quad|1\rangle=\left(\begin{array}{l}
0 \\
1
\end{array}\right)
$$
Therefore, the state of the qubit is the two dimensional complex vector  
$$
\begin{aligned}
|\phi\rangle=&\alpha|0\rangle+\beta|1\rangle \\
=&\alpha\left(\begin{array}{l}
1 \\
0
\end{array}\right)+\beta \left(\begin{array}{l}
0 \\
1
\end{array}\right)\\
=& \left(\begin{array}{c}
\alpha \\
\beta
\end{array}\right)
\end{aligned}
$$
where $\alpha$ and $\beta$ are the probability amplitudes. When we measure the qubit in the standard basis

- the probability of outcome $|0\rangle$ with value "0" is $|\alpha|^2$ 
- the probability of outcome $|1\rangle$ with value "1" is $|\beta|^2$

the normalization constraint means
$$
|\alpha|^2+|\beta|^2=1
$$

## Multi-qubit

Given the individual qubits, it is necessary to know how to construct the combined state of qubits. 

Suppose,  we have two single qubit states $|a\rangle=\left(\begin{array}{l}\alpha \\ \beta\end{array}\right)$ and $\left|b\right\rangle=\left(\begin{array}{l}\alpha^{\prime} \\ \beta^{\prime}\end{array}\right)$. The joint state of a system composed of two independent qubits is given by
$$
|a\rangle \otimes|b\rangle=\left(\begin{array}{c}
\alpha \\
\beta
\end{array}\right) \otimes\left(\begin{array}{c}
\alpha^{\prime} \\
\beta^{\prime}
\end{array}\right)=\left(\begin{array}{c}
\alpha \alpha^{\prime} \\
\alpha \beta^{\prime} \\
\beta \alpha^{\prime} \\
\beta \beta^{\prime}
\end{array}\right)
$$
Where we taking the tensor product $\otimes$ of two states. The tensor product of two states also can be denote as

- $|a\rangle|b\rangle$ 
- $|a b\rangle$ 

For example, The joint state of $|0\rangle$ and $|1\rangle$
$$
|00\rangle,|01\rangle,|10\rangle,|11\rangle
$$
The tensor product of these vectors are usually denoted as
$$
|00\rangle=\left(\begin{array}{l}
1 \\
0
\end{array}\right) \otimes\left(\begin{array}{l}
1 \\
0
\end{array}\right)=\left(\begin{array}{l}
1 \\
0 \\
0 \\
0
\end{array}\right)
$$

$$
|01\rangle=\left(\begin{array}{l}
1 \\
0
\end{array}\right) \otimes\left(\begin{array}{l}
0 \\
1
\end{array}\right)=\left(\begin{array}{l}
0 \\
1 \\
0 \\
0
\end{array}\right)
$$

$$
|10\rangle=\left(\begin{array}{l}
0 \\
1
\end{array}\right) \otimes\left(\begin{array}{l}
1 \\
0
\end{array}\right)=\left(\begin{array}{l}
0 \\
0 \\
1 \\
0
\end{array}\right)
$$

$$
|11\rangle=\left(\begin{array}{l}
0 \\
1
\end{array}\right) \otimes\left(\begin{array}{l}
0 \\
1
\end{array}\right)=\left(\begin{array}{l}
0 \\
0 \\
0 \\
1
\end{array}\right)
$$

A four-dimensional linear vector space is formed by these vectors basis, which could represent any two qubits. 
$$
|\psi\rangle=\alpha_1 |00\rangle +\alpha_2|01\rangle+\alpha_3|10\rangle+\alpha_4|11\rangle
$$


## Entangled states

A system of two qubits whose complete state can be the tensor product of two different single qubit states. An example of such a state is
$$
|01\rangle=\left(\begin{array}{l}
1 \\
0
\end{array}\right) \otimes\left(\begin{array}{l}
0 \\
1
\end{array}\right)=\left(\begin{array}{l}
0 \\
1 \\
0 \\
0
\end{array}\right)
$$
But it is possible for a state of two qubits that cannot be written as the tensor product of two single qubit states. For two qubits, these states are Bell states 
$$
\begin{aligned}
\left|\Phi^{\pm}\right\rangle &=\frac{1}{\sqrt{2}}(|00\rangle \pm |11\rangle)\\
\left|\Psi^{\pm}\right\rangle &=\frac{1}{\sqrt{2}}(|01\rangle \pm |10\rangle) 
\end{aligned}
$$
States of a system of which cannot be expressed as a tensor product of states of its individual subsystems are called **entangled states.** 

# Qubit gate

Mathematically, Quantum state is a vector $|\psi\rangle$. Qubits undergo unitary transformations to change their state.
$$
|\phi\rangle \rightarrow U_{1}|\phi\rangle \rightarrow U_{1}U_{2}|\phi\rangle \rightarrow U_{1}U_{2}\dots|\phi\rangle
$$
A unitary transformation is described by a matrix $U$ with complex entries. It is necessary for the matrix to be unitary due to the conservation of probabilities. 
$$
U U^{\dagger}=U^{\dagger} U=I,
$$
For example, a single qubit gate $U$  acting on a qubit state $|\phi\rangle=\alpha|0\rangle+\beta|1\rangle$ get a new state $|\psi\rangle$
$$
\begin{aligned}
|\psi\rangle=&U|\phi\rangle\\
=&\left(\begin{array}{ll}
U_{00} & U_{01} \\
U_{10} & U_{11}
\end{array}\right)\left(\begin{array}{l}
\alpha \\
\beta
\end{array}\right)\\
=&\left(\begin{array}{l}
U_{00} \alpha+U_{01} \beta \\
U_{10} \alpha+U_{11} \beta
\end{array}\right)
\end{aligned}
$$
Since quantum gates must be unitary operators, we often use the words "gate" and "operator" back and forth. 

$$
\text{Gate}= \text{Operator}
$$
So remember, in this note, they mean the same.

## Single qubit gate

1. NOT gate
2. Hadamard gate
3. Phase gate

### NOT gate

Start with the simplest quantum gate, the quantum NOT gate. In fact, we have seen the quantum NOT gate, that is, the $X $ Pauli matrix.
$$
X=U_{N O T}=\left(\begin{array}{ll}
0 & 1 \\
1 & 0
\end{array}\right)
$$
quantum NOT gate $U_ {N O T} $ acts on the computational basis
$$
|0\rangle=\left(\begin{array}{l}
1 \\
0
\end{array}\right), \quad|1\rangle=\left(\begin{array}{l}
0 \\
1
\end{array}\right)
$$
That are
$$
\begin{array}{l}
U_{N O T}|0\rangle=\left(\begin{array}{ll}
0 & 1 \\
1 & 0
\end{array}\right)\left(\begin{array}{l}
1 \\
0
\end{array}\right)=\left(\begin{array}{l}
0 \\
1
\end{array}\right)=|1\rangle \\
U_{N O T}|1\rangle=\left(\begin{array}{ll}
0 & 1 \\
1 & 0
\end{array}\right)\left(\begin{array}{l}
0 \\
1
\end{array}\right)=\left(\begin{array}{l}
1 \\
0
\end{array}\right)=|0\rangle
\end{array}
$$

#### new denote

We use the most intuitive way, the matrix to express the quantum gate and its effect on quantum bits. Now introduce the new denote
$$
X|j\rangle=|j \oplus 1\rangle
$$
where

- $|j\rangle$ Dirac notation of qubit
- $\oplus$ is exclusive-OR operator (XOR)

There are two possibilities

1. if $j=0,$ then $X|0\rangle=|0 \oplus 1\rangle=|1\rangle .$ 
2. if $j=1,$ then $X|1\rangle=|1 \oplus 1\rangle=|0\rangle$



### Hadamard gate

Hadamard matrix
$$
H=\frac{1}{\sqrt{2}}\left(\begin{array}{cc}
1 & 1 \\
1 & -1
\end{array}\right)
$$
Hadamard gate in the outer product notation
$$
H=\frac{1}{\sqrt{2}}(|0\rangle\langle 0|+| 0\rangle\langle 1|+| 1\rangle\langle 0|-| 1\rangle\langle 1|)
$$
Hadamard gate acts on stste $|0\rangle$ 
$$
\begin{aligned}
H|0\rangle &=\frac{1}{\sqrt{2}}(|0\rangle\langle 0|+| 0\rangle\langle 1|+| 1\rangle\langle 0|-| 1\rangle\langle 1|)|0\rangle \\
&=\frac{1}{\sqrt{2}}(|0\rangle\langle 0 | 0\rangle+|0\rangle\langle 1 | 0\rangle+|1\rangle\langle 0 | 0\rangle-|1\rangle\langle 1 | 0\rangle) \\
&=\frac{|0\rangle+|1\rangle}{\sqrt{2}}
\end{aligned}
$$
similarly
$$
H|1\rangle=\frac{|0\rangle-|1\rangle}{\sqrt{2}}
$$
Hadamard matrix acts on the standard basis states (computational basis states) to get two superposition states  
$$
\left\{\frac{|0\rangle+|1\rangle}{\sqrt{2}}, \frac{|0\rangle-|1\rangle}{\sqrt{2}}\right\}
$$

Therefore, the Hadamard gate takes the general state $|\psi\rangle=\alpha|0\rangle+\beta|1\rangle$ into the state
$$
\begin{aligned}
H|\psi\rangle &=\alpha H|0\rangle+\beta H|1\rangle\\
&=\alpha\left(\frac{|0\rangle+|1\rangle}{\sqrt{2}}\right)+\beta\left(\frac{|0\rangle-|1\rangle}{\sqrt{2}}\right) \\
&=\left(\frac{\alpha+\beta}{\sqrt{2}}\right)|0\rangle+\left(\frac{\alpha-\beta}{\sqrt{2}}\right)|1\rangle
\end{aligned}
$$


### Phase shift 

Generally, the phase shift gate is given by
$$
P=\left(\begin{array}{cc}
1 & 0 \\
0 & e^{i \theta}
\end{array}\right)
$$
the action of the phase shift gate on a qubit is
$$
P|\psi\rangle=\left(\begin{array}{cc}
1 & 0 \\
0 & e^{i \theta}
\end{array}\right)\left(\begin{array}{l}
\alpha \\
\beta
\end{array}\right)=\left(\begin{array}{c}
\alpha \\
e^{i \theta} \beta
\end{array}\right)
$$
We can find that this gate shifts the relative phase of the amplitudes $\alpha$ and $\beta$ of a qubit. This is more obvious if we write in other notation
$$
\begin{aligned}
P|\psi\rangle &=\left(|0\rangle\left\langle 0\left|+e^{i \gamma}\right| 1\right\rangle\langle 1|\right)\left(\cos \theta|0\rangle+e^{i \phi} \sin \theta|1\rangle\right) \\
&=\cos \theta|0\rangle+e^{i(\gamma+\phi)} \sin \theta|1\rangle
\end{aligned}
$$
where

- $|\psi\rangle=\cos \theta|0\rangle+e^{i \phi} \sin \theta|1\rangle$ the qubit in the Bloch sphere representation
- $P=|0\rangle\left\langle 0\left|+e^{i \gamma}\right| 1\right\rangle\langle 1|$ The phase shift operator in outer product notation

There are special cases of interest in we take special value of angle  $\theta$

1. when $\theta=\pi$

$$
\left(\begin{array}{cc}
1 & 0 \\
0 & e^{i \theta}
\end{array}\right)=\left(\begin{array}{cc}
1 & 0 \\
0 & e^{i \pi}
\end{array}\right)
=\left(\begin{array}{cc}
1 & 0 \\
0 & -1
\end{array}\right)
$$

In fact, this is one of the Pauli matrices, called the $Z$-matrix. The $Z$ matrix is sometimes called the phase flip gate because it takes a qubit $|\psi\rangle=\alpha|0\rangle+\beta|1\rangle$ into a state $\left|\psi^{\prime}\right\rangle=\alpha|0\rangle-\beta|1\rangle$. 
$$
Z|\psi\rangle=\left(\begin{array}{cc}
1 & 0 \\
0 & -1
\end{array}\right)\left(\begin{array}{l}
\alpha \\
\beta
\end{array}\right)=\left(\begin{array}{c}
\alpha \\
-\beta
\end{array}\right)
$$

2. when $\theta=\pi / 2 $

$$
\left(\begin{array}{cc}
1 & 0 \\
0 & e^{i \pi/2}
\end{array}\right)=\left(\begin{array}{ll}
1 & 0 \\
0 & i
\end{array}\right)=S
$$

3. when $\theta=\pi / 4$
   $$
   \left(\begin{array}{cc}
   1 & 0 \\
   0 & e^{i \pi / 4}
   \end{array}\right)=e^{i \pi / 8}\left(\begin{array}{cc}
   e^{-i \pi / 8} & 0 \\
   0 & e^{i \pi / 8}
   \end{array}\right)=T
   $$

This is called the $\pi / 8$ or $T$ gate:

## Two qubit gate

We will introduce a type of quantum gate called controlled gate. We can implement an if-else construct with a quantum gate using a controlled gate. Consider a control bit $C$. 

- If $C=0$, then the gate does nothing
- if $C=1$, then the gate performs some specified action. 

Generally, the controlled application of a single-qubit unitary $U$ to the second qubit takes the form
$$
\left(\begin{array}{ll}
I_{2} & 0_{2} \\
0_{2} & U
\end{array}\right) .
$$

### Controlled NOT gate

The first example we will introduce is the controlled NOT gate. The matrix representation of controlled NOT gate is given by
$$
C N=\left(\begin{array}{cccc}
1 & 0 & 0 & 0 \\
0 & 1 & 0 & 0 \\
0 & 0 & 0 & 1 \\
0 & 0 & 1 & 0
\end{array}\right)
$$
the action of the controlled NOT gate is given by
$$
\begin{array}{l}
C N|00\rangle \mapsto|00\rangle \\
C N|01\rangle \mapsto|01\rangle \\
C N|10\rangle \mapsto|11\rangle \\
C N|11\rangle \mapsto|10\rangle
\end{array}
$$
we can find 

- If the control qubit is $|0\rangle$, then nothing happens to the target qubit. 
- If the control qubit is $|1\rangle$, then the NOT matrix is applied to the target qubit. 

The outer product representation of the controlled NOT with Dirac notation is
$$
C N=|00\rangle\langle 00|+| 01\rangle\langle 01|+| 10\rangle\langle 11|+| 11\rangle\langle 10|
$$
the action of the controlled NOT gate on $|10\rangle$ in detail 
$$
\begin{aligned}
C N|10\rangle &=(|00\rangle\langle 00|+| 01\rangle\langle 01|+| 10\rangle\langle 11|+| 11\rangle\langle 10|)|10\rangle \\
&=|00\rangle\langle 00|| 10\rangle+|01\rangle\langle 01|| 10\rangle+|10\rangle\langle 11 ||10\rangle+|11\rangle\langle 10 ||10\rangle
\\&=|11\rangle
\end{aligned}
$$

where we use 
$$
\begin{aligned}
\langle 00 | 10\rangle &=\langle 0 | 1\rangle\langle 0 | 0\rangle=0 \\
\langle 01 | 10\rangle &=\langle 0 | 1\rangle\langle 1 | 0\rangle=0 \\
\langle 11 | 10\rangle &=\langle 1 | 1\rangle\langle 1 | 0\rangle=0 \\
\langle 10 | 10\rangle &=\langle 1 | 1\rangle\langle 0 | 0\rangle=1
\end{aligned}
$$


### Controlled-Hadamard gate

The matrix representation of the controlled Hadamard gate is
$$
C H=\left(\begin{array}{cccc}
1 & 0 & 0 & 0 \\
0 & 1 & 0 & 0 \\
0 & 0 & \frac{1}{\sqrt{2}} & \frac{1}{\sqrt{2}} \\
0 & 0 & \frac{1}{\sqrt{2}} & \frac{-1}{\sqrt{2}}
\end{array}\right)
$$

1. applied to the state $|01\rangle$
   $$
   C H|01\rangle=\left(\begin{array}{cccc}
   1 & 0 & 0 & 0 \\
   0 & 1 & 0 & 0 \\
   0 & 0 & \frac{1}{\sqrt{2}} & \frac{1}{\sqrt{2}} \\
   0 & 0 & \frac{1}{\sqrt{2}} & \frac{-1}{\sqrt{2}}
   \end{array}\right)\left(\begin{array}{l}
   0 \\
   1 \\
   0 \\
   0
   \end{array}\right)=\left(\begin{array}{l}
   0 \\
   1 \\
   0 \\
   0
   \end{array}\right)=|01\rangle
   $$

The output is $|01\rangle$. The $C H$ gate does nothing, since the control qubit is set to 0 .  

2. applied to the state $|11\rangle$

$$
C H|11\rangle=\left(\begin{array}{cccc}
1 & 0 & 0 & 0 \\
0 & 1 & 0 & 0 \\
0 & 0 & \frac{1}{\sqrt{2}} & \frac{1}{\sqrt{2}} \\
0 & 0 & \frac{1}{\sqrt{2}} & \frac{-1}{\sqrt{2}}
\end{array}\right)\left(\begin{array}{l}
0 \\
0 \\
0 \\
1
\end{array}\right)=\left(\begin{array}{c}
0 \\
0 \\
\frac{1}{\sqrt{2}} \\
\frac{-1}{\sqrt{2}}
\end{array}\right)
$$

The control qubit is 1. Then the target qubit has been taken by Hadamard gate.



## Decomposition and universal gate set

### Decomposition of single qubit gate

1. Z-Y-Z decomposition
2. General Euler angle decompositions
3. Bloch rotation decomposition
4. Decomposition of Bloch rotation

#### Z-Y-Z decomposition

Any single qubit gate can be decomposed as a sequence of $Z, Y$, and $Z$ rotations, and a phase
$$
U=e^{i \alpha} R_z\left(\theta_2\right) R_y\left(\theta_1\right) R_z\left(\theta_0\right)
$$

#### General Euler angle decompositions

Any single qubit gate can be decomposed as a sequence of $X, Y$, and $X$ rotations
$$
\mathrm{U}=\mathrm{R}_{\mathrm{x}}\left(\theta_2\right) \mathrm{R}_{\mathrm{y}}\left(\theta_1\right) \mathrm{R}_{\mathrm{x}}\left(\theta_0\right)
$$

####  Bloch rotation decomposition

decompositions of single-qubit gates into single rotations about a particular axis 
$$
R_{\vec{n}}(\theta)=\left[\begin{array}{cc}
\cos \left(\frac{1}{2} \theta\right)-i n_z \sin \left(\frac{1}{2} \theta\right) & -n_y \sin \left(\frac{1}{2} \theta\right)-i n_x \sin \left(\frac{1}{2} \theta\right) \\
n_y \sin \left(\frac{1}{2} \theta\right)-i n_x \sin \left(\frac{1}{2} \theta\right) & \cos \left(\frac{1}{2} \theta\right)+\operatorname{in}_z \sin \left(\frac{1}{2} \theta\right)
\end{array}\right]
$$

#### Decomposition of Bloch rotation

A rotation about an arbitrary axis in the Bloch sphere can be analytically decomposed into a sequence of five $R_z$ and $R_y$ gates 
$$
R_{\vec{n}}(\theta)=R_z(+\alpha) R_y(+\beta) R_z(\theta) R_y(-\beta) R_z(-\alpha)
$$

### Decomposition of two-qubit gates

1. Kronecker decomposition
2. Canonical decomposition
3. CNot decomposition
4. ABC decomposition

### universal gate set

For quantum computers, a gate set is called universal if the gates therein can be used to approximate any unitary transformation on any number of qubits to any desired precision. 

1. A common universal gate set is the Clifford + T gate set, which is composed of the $\{\mathrm{CNOT}, H, S\}$ and $T$ gates. 
2. One universal gate set is $\{\mathrm{CNOT}, H, T\}$. 
3. Deutsch's gate, i.e. controlled-controlled-i $i R_x(a \pi)$, is universal whenever $a$ is irrational.

The Clifford set alone is not universal and can be efficiently simulated classically by the Gottesman–Knill theorem.

Gottesman-Knill theorem: A quantum circuit using only the following elements  $\{\mathrm{CNOT}, H, S\}$ can be simulated efficiently on a classical computer











