## 1 Node Embeddings with TransE 

### 1.1 Simplified Objective
Lets consider a graph consisting of just 2 nodes $h$ and $g$ and a directed edge netween them $l$, such that true relationship shpuld sutisfy $h+l = g$ with non-trivial emdebiings vectors. For example we could have: $h=(0,1)$, $l=(1,-1)$, $t = (1,0)$. 

<p align="center">
<img src="../pictures/2-node-graph.png" alt="Raspberry pi" style="width:20%; border:0;">
</p>

However, one can easily check that the simplified objective function could go down to zero for any values of $h$ and $t$ with $l=0$. Those embeddings values do not make sence. 
 