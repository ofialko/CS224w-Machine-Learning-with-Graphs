## 1 GraphRNN  

### 1.1 Edge-level RNN
* 1st node added is A.
* 2nd node added is B: RNN predicts it has edge with node A.
* 3rd node added is D: RNN predicts it has edge with node A.
* 4th node added is C: RNN predicts it has edge with node B.
* 5th node added is E: RNN predicts it has edges with B and D.
* 6th node added is F: RNN predicts it has edges with C and E.
 
### 1.2 Advantages of BFS ordering

As discussed in the lecture, random BFS ordering limits the complexity of a random ordering of nodes:
* Reduce possible node orderings
* Reduce steps for edge generation

## 2 LightGCN 

### 2.1 Advantages of Average Embeddings

This includes embeddings diffused at multiple hop scales. This takes into account local as well global connectivity patterns for a given node,encouraging the embeddings of similar users/items to be similar. 

### 2.2 Self-connection

One can say that self-connection is implicit at the second hop level. For example, at this level a user embedding is a sum from self-connection and aggreagted contribution of other users (connected to the current user via common items). For this reason and simplification purpose of LGCN, explicit self-connection is not required.

### 2.3 Relation with APPNP

$$
\bf E^{(K)} = \beta E^{0} + (1-\beta)\tilde{A}E^{(K-1)} = 
$$

$$
\bf \beta E^{0} + (1-\beta)\tilde{A}(\beta E^{0} + (1-\beta)\tilde{A}E^{(K-2)}) = 
$$

$$
\bf \beta(1 + (1-\beta)\tilde{A}) E^{0} + (1-\beta)^2\tilde{A}^2E^{(K-2)} = 
$$

$$
...  
$$

$$
\bf = \beta \sum_{k=0}^K (1-\beta)^k \tilde{A}^k E^{0}
$$

### 2.4 Recommendation Task