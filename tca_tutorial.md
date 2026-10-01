# What is a tensor?

A tensor is the mathematical analog of a numpy `ndarray`, and is a generalization of a *matrix*.

$\newcommand{\data}{\mathcal{D}}$
A tensor $\data \in \mathcal{R}^{d1 \times d2 \times ... \times dn}$ is an *order n* tensor. A vector has order 1, and a matrix has order 2.

In this tutorial, we will be concerned with the order 3 tensor formed by collecting neural data from $N$ neurons at $T$ time points, across $K$ trials, i.e. we will have $\data \in \mathcal{R}^{N \times T \times K}$, and $\data_{n, t, k}$ will indicate the data (spike or rate information) collected from the $n$-th neuron on the $t$-th timestep on the $k$-th trial.

#Tensor Component Analysis

We want to approximate the data tensor $\data$ with a *simpler* tensor $\hat \data$ composed of components that are lower dimensional and easier to interpret. In analogy to low-rank matrix factorizations (e.g. PCA), we can suppose that the data matrix will be well-captured by $R$ sets of vectors, $\textbf{a}_r$, $\textbf{b}_r$, and $\textbf{c}_r$. Vectors $\textbf{a}$ are $N$-dimensional and dictate which neurons participate in the set, vectors $\textbf{b}_r$ are $T$-dimensional and dictate how the neurons' activity changes over time, and vecotrs $\textbf{c}_r$ are $K$-dimensional and dictate how the neurons' activity changes across trials.

We then approximate the data tensor as:

$
\hat \data = \sum_{r = 1}^R \textbf{a}_r \circ \textbf{b}_r \circ \textbf{c}_r,
$

Where $\cdot$ denotes the *tensor outer product*. For a particular tensor index $n,t,k$, this gives:

$
\hat \data_{n,t,k} = \sum_{r = 1}^R (a_n)_r (b_t)_r (c_k)_r.
$

We can organize our vectors into factor matrices:

$
\mathbf{A} = \begin{pmatrix} | & ... & | \\ \mathbf{a}_1 & ... & \mathbf{a}_R \\  | & ... & | \end{pmatrix} ~~~
\mathbf{B} = \begin{pmatrix} | & ... & | \\ \mathbf{b}_1 & ... & \mathbf{b}_R \\  | & ... & | \end{pmatrix} ~~~
\mathbf{C} = \begin{pmatrix} | & ... & | \\ \mathbf{c}_1 & ... & \mathbf{c}_R \\  | & ... & | \end{pmatrix}
$

And then we want to solve the minimization problem:

$
\argmin_{\mathbf{A},\mathbf{B},\mathbf{C}} \| \data - \hat \data \|_2^2 
$

$
= \argmin_{\mathbf{A},\mathbf{B},\mathbf{C}} \sum_{n,t,k} \left ( \data - \hat \data_{n,t,k} \right )^2 
$

$
=\argmin_{\mathbf{A},\mathbf{B},\mathbf{C}} \sum_{n,t,k} \left ( \data - \sum_{r=1}^R  \textbf{a}_r \circ \textbf{b}_r \circ \textbf{c}_r \right )^2.
$

This would be very difficult to optimize in $\mathbf{A}$, $\mathbf{B}$, and $\mathbf{C}$ simulataneously (the optimization is non-convex), but we can observe that the problem *is* convex in $\mathbf{A}$ is both $\mathbf{B}$ and $\mathbf{C}$ are held fixed. In fact, it is equivalent to a linear regression problem (see code for details)!

# Alternating Least Squares (ALS)

The alternating least squares algorithm performs this optimization by *coordinate descent*. First we optimize for $\mathbf{A}$, then we optimize for $\mathbf{B}$, then for $\mathbf{C}$, performing linear regression at each stage. This iteration scheme is then repeated until convergence. The TCA method additionally includes a normalization step to prevent degeneracy between the vectors $\mathbf{a}_r$, $\mathbf{b}_r$, and $\mathbf{c}_r$ (see code for details). In Pseudocode this gives us:


## Algorithm: Alternating Least Squares (ALS)

### INPUT: 
Tensor $\data$ and target rank $R$
### Output: 
$\mathbf{A}$, $\mathbf{B}$, $\mathbf{C}$ each with R columns used to fit D

Initialize $\mathbf{A},\mathbf{B},\mathbf{C}$ randomly

### Repeat until convergence:

$\mathbf{A} \leftarrow \argmin_a \| \data - \hat \data \|_2^2$

$\mathbf{B} \leftarrow \argmin_a \| \data - \hat \data \|_2^2$

$\mathbf{C} \leftarrow \argmin_a \| \data - \hat \data \|_2^2$

Renormalize($\mathbf{A}$,$\mathbf{B}$,$\mathbf{C}$)

Return $\mathbf{A}$,$\mathbf{B}$,$\mathbf{C}$