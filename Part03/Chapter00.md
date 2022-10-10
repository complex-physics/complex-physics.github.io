To be continued

Baisc Algorithms



<!-- toc -->







building block

[TOC]







Reference

1. Mikio Nakahara, Tetsuo Ohmi.《Quantum Computing. From Linear Algebra to Physical Realizations 》
2. David McMahon. 《Quantum Computing Explained 》
3. Michael A Nielsen and Isaac Chuang.《Quantum computation and quantum information》, 2002
4. Ivan B. Djordjevic《Quantum Information Processing, Quantum Computing, and Quantum Error Correction: An Engineering Approach》



# Quantum Fourier transform

Given a state
$$
|\psi\rangle=\sum_{j=0}^{N-1} a_j|j\rangle=\left(\begin{array}{c}
a_0 \\
\vdots \\
a_{N-1}
\end{array}\right)
$$
the quantum Fourier transform $U_{Q F T}$ acts on a quantum state $|\psi\rangle$ and maps it to the quantum state $|\tilde{\psi}\rangle$
$$
\begin{aligned}
|\tilde{\psi}\rangle=U_{Q F T}|\psi\rangle
\end{aligned}
$$
the QFT map can be represnt by a matrix
$$
U_{N}^{Q F T}=\frac{1}{\sqrt{N}}\left(\begin{array}{cccccc}
1 & 1 & 1 & 1 & \cdots & 1 \\
1 & \omega & \omega^2 & \omega^3 & \cdots & \omega^{N-1} \\
1 & \omega^2 & \omega^4 & \omega^6 & \cdots & \omega^{2(N-1)} \\
1 & \omega^3 & \omega^6 & \omega^9 & \cdots & \omega^{3(N-1)} \\
\vdots & \vdots & \vdots & \vdots & & \vdots \\
1 & \omega^{N-1} & \omega^{2(N-1)} & \omega^{3(N-1)} & \cdots & \omega^{(N-1)(N-1)}
\end{array}\right)
$$






### Example: 1-qubit QFT

the orthonormal basis $\{|j\rangle\}=\{|0\rangle ,| 1\rangle \} $ transformed into
$$
\begin{aligned}
|0\rangle & \rightarrow \frac{1}{\sqrt{2}} \sum_{k=0}^1 e^{i 2 \pi \cdot 0 \cdot k / 2}|k\rangle=\frac{1}{\sqrt{2}}(|0\rangle+|1\rangle)\\
|1\rangle & \rightarrow \frac{1}{\sqrt{2}} \sum_{k=0}^1 e^{i 2 \pi \cdot 1 \cdot k / 2}|k\rangle=\frac{1}{\sqrt{2}}(|0\rangle-|1\rangle)
\end{aligned}
$$
acts on a single qubit state $|\psi\rangle=\alpha|0\rangle+\beta|1\rangle$
$$
\begin{aligned}
U_{Q F T}|\psi\rangle=& U_{Q F T} (\alpha|0\rangle+\beta|1\rangle)
\\=&\frac{1}{\sqrt{2}}(\alpha+\beta)|0\rangle+\frac{1}{\sqrt{2}}(\alpha-\beta)|1\rangle
\end{aligned}
$$

$$
\begin{aligned}
U_{Q F T}|\psi\rangle=& U_{Q F T} (\alpha|0\rangle+\beta|1\rangle)
\\=&\alpha\frac{1}{\sqrt{2}}\left[\begin{array}{cc}
1 & 1 \\
1 & -1
\end{array}\right]
\left(\begin{array}{c}
1 \\
0
\end{array}\right)+\beta\frac{1}{\sqrt{2}}\left[\begin{array}{cc}
1 & 1 \\
1 & -1
\end{array}\right]
\left(\begin{array}{c}
0 \\
1
\end{array}\right)
\\=&\frac{1}{\sqrt{2}}(\alpha+\beta)\left(\begin{array}{l}
1 \\
0
\end{array}\right)+\frac{1}{\sqrt{2}}(\alpha-\beta)\left(\begin{array}{l}
0 \\
1
\end{array}\right)
\\=&\frac{1}{\sqrt{2}}(\alpha+\beta)|0\rangle+\frac{1}{\sqrt{2}}(\alpha-\beta)|1\rangle
\end{aligned}
$$


### Examples 2-qubit QFT

the orthonormal basis of 2-qbit system

- $\left.|0\rangle=\left|0_1 \rangle \right| 0_0\right\rangle$
- $|1\rangle =| 0_1 \rangle | 1_0\rangle$
- $|2\rangle =| 1_1 \rangle | 0_0 \rangle$
- $|3\rangle=\left|1_1\rangle \right| 1_0 \rangle$

the orthonormal basis transformed into
$$
\begin{aligned}
|0\rangle & \rightarrow \frac{1}{\sqrt{4}} \sum_{k=0}^3 e^{i 2 \pi \cdot 0 \cdot k / 4}|k\rangle=\frac{1}{2}(|0\rangle+|1\rangle+|2\rangle+|3\rangle)
\\
|1\rangle & \rightarrow  \frac{1}{\sqrt{4}} \sum_{k=0}^3 e^{i 2 \pi \cdot 1 \cdot k / 4}|k\rangle=\frac{1}{2}(|0\rangle+i|1\rangle-|2\rangle-i|3\rangle)
\\
|2\rangle & \rightarrow \frac{1}{\sqrt{4}} \sum_{k=0}^3 e^{i 2 \pi \cdot 2 k \cdot / 4}|k\rangle=\frac{1}{2}(|0\rangle-|1\rangle+|2\rangle-|3\rangle)
\\
|3\rangle & \rightarrow \frac{1}{\sqrt{4}} \sum_{k=0}^3 e^{i 2 \pi \cdot 3 \cdot k / 4}|k\rangle=\frac{1}{2}(|0\rangle-i|1\rangle-|2\rangle+i|3\rangle) 
\end{aligned}
$$
2-qubit QFT in matrix form:
$$
\begin{aligned}
F=&\frac{1}{2}\left(\begin{array}{cccc}
1 & 1 & 1 & 1 \\
1 & \omega & \omega^2 & \omega^3 \\
1 & \omega^2 & 1 & \omega^2 \\
1 & \omega^3 & \omega^2 & \omega
\end{array}\right)
\\=& \frac{1}{2}\left(\begin{array}{cccc}
1 & 1 & 1 & 1 \\
1 & \mathrm{i} & -1 & -\mathrm{i} \\
1 & -1 & 1 & -1 \\
1 & -\mathrm{i} & -1 & \mathrm{i}
\end{array}\right)
\end{aligned}
$$
where

- $\omega=\mathrm{e}^{\pi \mathrm{i} / 2}$

### binary number expression



To be continued
