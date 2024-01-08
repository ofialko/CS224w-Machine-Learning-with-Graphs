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
It is a probability distribution, $\sum_i r_i = 1$.

## 1.3 Relation to Random Walk (i)
The random walk transition matix:
$$
M = D^{-1}A
$$
One can show that each row is normalized, $\forall i: \sum_j M_{ij} = 1$, so it has proper probabilistic property.

## 1.4 Relation to Random Walk (ii)
The random walk transition matix with skip connection:
$$
M = \frac{1}{2}I + \frac{1}{2}D^{-1}A
$$
Likewise to the previous case, $\forall i: \sum_j M_{ij} = 1$.


## 1.5 Over-Smoothing Effect
We need to apply the random walk transition matix infinite number of times to an arbitrary initial state vector $r$ and investigate the resulting state $r'$:
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
So, the resulting state $r'$ has all identical components regardless of the initial state $r$ distrbition. 

We can generalize this to embeddings matrix $H$ with each row containing nodes embeddings:
$$
h'_{ij} = \lim_{l\to\infty}(M^{l} H)_{ij} \approx \sum_{k}(v_1\cdot v_1^T)_{ik} H_{kj} = \frac{1}{n}\sum_k H_{kj}
$$
Thus resulting embedding vectors for all nodes $i$ are identical having components $j$ as shown in the above formular.

## 1.6 Learning BFS with GNN

BFS algorythm:
* Source node $𝑠$
* Initial input: 
$$h_i^{(0)}=\begin{cases}
1 & \quad \text{if  $i=s$}\\ 
0 & \quad \text{otherwise}
\end{cases}
$$
* Node is reachable from 𝑠 if any of its neighbors are reachable:
$$h_i^{(t+1)}=\begin{cases}
1 & \quad \text{if  $h_i^{(t)}=1$}\\ 
1 & \quad \text{if  $\exists j$ s.t. $(i,j)\in E$ and $h_j^{(t)}=1$}\\ 
0 & \quad \text{otherwise}
\end{cases}
$$

This can be rewriten in matrix notation.
Initially, the embedding matrix is diagonal: 
$$
H^{(0)} = \hat{I}
$$
After first step, each node gets updated with visits from each nearest neigbour:
$$
H^{(1)} = \hat{I} + A = (\hat{I} + A)H^{(0)}
$$
After $t+1$ steps:
$$
H^{(t+1)} = \theta\left((\hat{I}+ A)H^{(t)}\right)
$$
where
$$
\theta(x) = \begin{cases}
1 & \quad \text{if  $x>0$}\\ 
0 & \quad \text{otherwise}
\end{cases}
$$
to ensure that visited nodes get value 1 each. 

More explicetly, 
$$
h_{\nu}^{(l)} = \theta\left(h_{\nu}^{(l-1)} + \sum_{u\in N(\nu)}h_{u}^{(l-1)}\right)
$$
Messages are row embeddings, an aggregation is their sum, an update rule is applying $\theta$ to the sum in the brackets in the above formula.

## 2.1 Simple matrix factorization
Decoder
$$
DEC(z_{\nu}^Tz_u) = z_{\nu}^Tz_u
$$

## 2.2 Alternate matrix factorization
The optimization objective is
$$
\min_Z ||A-Z^TWZ||
$$

## 2.3 BONUS: Relation to eigen-decomposition
If $W$ is diagonal containing eigenvalues of $A$, then the above matrix decomposition is equivalent to learning the eigen-decomposition of matrix A.

## 2.4 Multi-hop node similarity

k-hop adjecency matrix is $A_k = A+A^2+...+A^k$. The optimization objective is
$$
\min_Z ||A_k-Z^TZ||
$$

## 2.5 node2vec & struct2vec (i)
