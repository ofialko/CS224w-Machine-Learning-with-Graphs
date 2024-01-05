## 1.1 Effect of Depth on Expressiveness 

Three message passing layers are needed to distinguish the two red nodes.

 Layer 1 aggregates information from identical nodes 5. Layer 2 reaches identical nodes 4 and 6. Layer 3 reaches different nodes in each graph, making them distinguishable.

 Thus, at least 3-layer GNN is necessary.

 ## 1.2 Random Walk Matrix
 ### 1
 The random walk transition matrix is
 $$
M=
\begin{bmatrix}
0 & 1/2 & 1/2 & 0\\
1/2 & 0 & 1/2 & 0\\
1/3 & 1/3 & 0 & 1/3\\
0 & 0 & 1 & 0
\end{bmatrix}
$$
### 2
The limiting distribution r, such that r=Mr, is
$$
r=(1/4,1/4,1/4,1/4)
$$

## 1.3 Relation to Random Walk (i)
The random walk transition matix:
$$
M = D^{-1}A
$$

## 1.4 Relation to Random Walk (ii)
The random walk transition matix with skip connection:
$$
M = \frac{1}{2}I + \frac{1}{2}D^{-1}A
$$

## 1.5 Over-Smoothing Effect
We need to apply the random matrix transition matrix infinite number of times to an arbitrary initial state vector $r$ and investigate the resulting state $r'$:
$$
r' = \lim_{l\to\infty}M^{l}r
$$
to show it has all identical components.

First, we can show that $v_1 = \frac{1}{\sqrt{n}} \hat{I}$ (n is the total number of nodes) is the eigenvector of the random walk matrix associated with eigenvalue $\lambda =1$:
$$
(Mv_1)_i= \frac{1}{\sqrt{n}}M\hat{I} = \frac{\sum_{j\in N(i)}\frac{1}{d_j}}{\sqrt{n}}= \frac{1}{\sqrt{n}} = (v_1)_i
$$
Contributions corresponding to eigenvalue $\lambda = 1$ will survive as $l$ grows. Using the spectral decompisition, we get:
$$
r' \approx (v_1\cdot v_1^T) r  = \frac{\sum_i r_i}{n} \hat{I} = \frac{1}{n} \hat{I}
$$
So, the resulting state has all identical components. This explains the over-smoothing effect.

