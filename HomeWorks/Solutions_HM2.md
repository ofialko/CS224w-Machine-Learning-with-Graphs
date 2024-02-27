## 1 Node Embeddings with TransE 

### 1.1 Simplified Objective
Lets consider a graph consisting of just 2 nodes $h$ and $g$ and a directed edge between them $l$, such that true relationship should sutisfy $h+l = g$ with non-trivial emdebiings vectors. For example we could have: $h=(0,1)$, $l=(1,-1)$, $t = (1,0)$. 

<p align="center">
<img src="../pictures/2-node-graph.png" alt="Raspberry pi" style="width:20%; border:0;">
</p>

However, one can easily check that the simplified objective function $\cal{L}_{\text{simple}}$
could go down to zero if $h=t$ and $l=0$ for any $h$ (or $t$). Those embeddings values do not make sence for the graph presented above. 
 
 ### 1.2 Utility of $\gamma$

 Lets consider a 3-node graph presented below. The node $t'$ is related neither to $h$ nor $t$.

<p align="center">
<img src="../pictures/2-node-graph-corrupted.png" alt="Raspberry pi" style="width:20%; border:0;">
</p>

The desired embeddings are $h=(0,1)$, $l=(1,-1)$, $t = (1,0)$ and $t'=(\sqrt{1/2},\sqrt{1/2})$. Easy to check that the corresponding objective function $\cal{L}_{\text{no margin}} =  0$. 

For any other values of embeddings such that $d(h+l,t) - d(h+l,t') < 0$ and all them being normalized appropriatelly, the objective function $\cal{L}_{\text{no margin}}=0$ as well. We just need to make sure that the vector $t'$ is further away from $h+l$ than the vector $t$. 

For instance, lets assume $h=(0,1)$, $l=(2,-1)$ (incorrect), $t = (1,0)$ and $t'=(\sqrt{1/2},\sqrt{1/2})$. Then those values will satisfy $\cal{L}_{\text{no margin}} = 0$ as well, since the distance from $A$ to $t$ is shorter than to $t'$.

<p align="center">
<img src="../pictures/2-node-graph-corrupted2.png" alt="Raspberry pi" style="width:20%; border:0;">
</p>

The purpose of the margin $\gamma > 0$ is to panalise the arbitrariness of embeddings values, by correcting the sign  $\gamma + d(h+l,t) - d(h+l,t') > 0$ making $\cal{L}_{\text{no margin}}>0$.

In our case, we need to make sure that the margin $\gamma$ is greater than 0.47 (wrong difference) but less than 0.765 (true difference).


### 1.3 Normalizing the embeddings 

For any values of $\gamma$, $h$, $t$ and $l$, there will always be a value $t'$ (with any arbitrary norm) to make the term $\gamma + d(h+l,t) - d(h+l,t') < 0$. Normalization of embeddings is necessary to preclude such behaviour.

### 1.4 Expressiveness of TransE embeddings 

## 2 Expressive Power of Knowledge Graph Embeddings

#### 2.1 TransE Modeling

* Symmetry: TransE can not model it, since that would imply $l=-l$, which would lead to $l=0$.

* Inverse: TransE can model it, since 2 different relations have two different embeddings such that $l_{\text{AB}} = -l_{\text{BA}}$

* Composition: Yes again, since $l_{\text{CA}} + l_{\text{AB}} = l_{\text{CB}}$.

#### 2.2 RotatE Modeling

* Symmetry: One would need to assure $h\circ l = t$ as well as $t \circ l = h$. This is possible provided angles $l$ are $180^\circ$, and the vectors $h$ and $t$ are opposite to each other, e.g. $h=-t$ in 2D space. 

* Inverse: Yes, since there will be 2 different rotations, such that $h\circ l_{\text{AB}} = t$ and $t\circ l_{\text{BA}} = h$.

* Composition: Yes again, rotations can be composed $l_{\text{CA}} \oplus l_{\text{AB}} = l_{\text{CB}}$

#### 2.3 Failure Cases

Both models can not model 1 to many relationships.

## 3 Queries on Knowledge Graphs

#### 3.1 Path Queries on Complete KGs
Query
$$
(e:\text{Arimidex}, (r:\text{treated}, r:\text{associated})))
$$

Answer: BRCA1, ESR1, ESR2, BIRC2

#### 3.2 Conjunctive Queries on Complete KGs 
Query
$$
((e:\text{Arimidex}, (r:\text{treated}, r:\text{associated})), (e:\text{Fulvestrand}, (r:\text{causes}, r:\text{associated})))
$$

#### 3.3 Incomplete KGs
Since DistMult cannot handle compositional relations, they cannot be easily extended to handle path queries.

#### 3.4 Query2box
Queries
$$
(e:A, (r:R_1, r:R_2)) - \text{red box}
$$
$$
(e:C, (r:R_3) - \text{green box}
$$

<p align="center">
<img src="../pictures/Query2Box.png" alt="Raspberry pi" style="width:50%; border:0;">
</p>
The answer is  (F,E,D), which sit in the intersection of the red and the green boxes. 

## 4 Subgraph and Order Embeddings

#### 4.1 Transitivity

A subgraph of B is induced by $\{f(v)|v ∈ V_A\}$, while subgraph of C is induced by  $\{g(u)|u ∈ V_B\}$, for two different bijective functions $f$ and $g$. It follows that there is a subgraph in C which is induced by a composite bijection  $\{g\circ f(v))|v ∈ V_A\}$. Assuming that the composition of two bijective functions is bijective, we conclude that A is a subgraph of C.

#### 4.2 Anti-symmetry

We can write for a subgraph of B $\{f(v)|v ∈ V_A\}$ and for a subgraph of A $\{g(u)|u ∈ V_B\}$. A subgraph of A can also be presented as a bijection $\{f\circ g(v))|v ∈ V_A\}$, so its maps to itself ($v ∈ V_A$ means every node in A). This implies that graph B maps to the entire A. Similarly, graph A maps to the entire graph B. That's why A and B are identical.

#### 4.3 Common Subgraphs

$z_X ≼ min\{z_A,z_B\}$ implies $z_X ≼ z_A$ and $z_X ≼ z_B$. So X is a common subraph of A and B. 

#### 4.4 Order Embedding Constraints

If $z_A[0] > z_B[0] > z_C[0]$, then  $z_A[1] < z_B[1] < z_C[1]$, since A,B,C are not subgraphs of each other.

#### 4.5 Subgraph Relations

We have $z_A[0] > z_B[0] > z_C[0]$ and $z_A[1] < z_B[1] < z_C[1]$.

If X is a subgraph of A,B,C; Y and Z are subgraphs of B only as shown in the picture below, then 
$$
z_X ≼ z_Y, z_X ≼ z_Z
$$
<p align="center">
<img src="../pictures/SubgraphsRelations-2.png" alt="Raspberry pi" style="width:50%; border:0;">
</p>