# Introduction
The goal of this note is to solve a new subclass of the graph isomorphism problem.
The strategy of this solution is to use a unique singular value decomposition to compute the permutation.
The runtime of this algorithm is far clearer than the correctness.
This algorithm works with undirected graphs having vertex or edge weights.

# Unique SVD
We start with the necessary assumption which defines the subclass:
#### Definition:The graph G is spectrally irregular iff
#### the adjacency matrix of G lacks a nontrivial eigenvector orthogonal to the vector of all 1s
Reminder: the trivial vector is the all zero vector.

#### Theorem: Spectral Irregular Graph -> Unique SVD of Adjacency matrix
For each eigenvalue of the adjacency matrix of G,
the space spanned by those eigenvectors has infinitely many orthogonal basises.
However, when the graph is spectrally irregular,
we can determine the basis vectors, by maximizing the inner product with the all 1s vector
while keeping the vector length 1; then,
picking the next vector orthogonal to the previous vectors, but still in the eigenspace.
For two equal eigenspaces, this will always result in the same sequence of vectors,
as although this might not define a convex program, 
the intersection of the vectorspace with the unit sphere in n dimensions gives a lower dimensional unit sphere,
and this lower dimensional sphere has a unique maximum of the function 1Tx when 1T is not orthogonal to it.

#### Algorithm: Finding the unique svd.
Using T to mean conjugate transpose:
We can find these vectors by formulating the problem as a Rayleigh Quotient.
If ```e_i``` is in the span of the eigenspace,
then we can write ```e_i = Ey``` where ```E``` is an orthogonal basis of the eigenspace.
then we can enforce ```e_jTe_i = 0``` by letting ```N_j = I-e_je_jT``` and 
then gathering the constraints into```N = Product j from 1 to i-1(N_j) ```
then ```e_i = NEy``` can be combined with the constraint ``` e_iTe_i = 1```
to form The following simple recursion:
find the only length 1 eigenvector ```y``` of ```ETNT11TNE``` then compute ```e_i = NEy```
and updating ```N```; return all e_i
This takes O(m^2n) work for each eigenspace of multiplicity m.
Thus given a nonspecific svd for a spectally irregular graph, we can get the unique svd.

# Isomorphism Problem for Spectrally Irregular Graphs
## Algorithm
```
Isomorphism(G,H)
  U,A = Unique_SVD(A_G)
  V,B = Unique_SVD(A_H)
  if A != B
    return false
  V_t = conjugate_transpose(V)
  return is_permutation(V_t*U)
```
## Runtime:
The runtime is cubic, because of computing the svd's; making them unique; and doing 1 matrix multiplication.
## Correctness:
If G and H are not cospectral, they are not isomorphic and so the cospectral test handles that case.
If G and H are isomorphic,
Then the ith unique eigenvector of A_G is permuted to the same eigenvector of A_H.
Because it must share an eigenvalue:
``` lu = A_Gu = PTA_HPu ``` therefore ```lPu = A_HPu```
And it must share the order with which it was chosen by the uniqueness process.
Thus ```VTU = P ``` where the columns of ```U``` are eigenvectors of ```A_G```.
Conversely,
```PTA_HP = (UTV)A_H(VTU) = (UTV)(VTDV)(VTU) = UTDU``` []

# Conclusion:
I would love to get running code and a bunch of tests before submitting this for publication.
I think this algorithm is fast enought to test easily, and simple enough to implement.
My main concern is I have not analyzed how much precision the svd needs to be computed with.
