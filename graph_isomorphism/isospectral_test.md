# Introduction
When two graphs are isomorphic,
they are not only isospectral, but all identical weightings of them remain isospectral.
Is this property strong enough to solve graph isomorphism? 
I don't know, but here is a strategy to exploit it.

# Algorithm
## Pseudocode
```
Graph_Isomorphism(G,H):
  if n(G) != n(H) :
    return NULL
  if n(G) == 1:
    return empty_list.append( ( V(G)[0], V(H)[0] ) )
  v := V(G)[0] //some arbitrary vertex of G
  for u in V(H):
    boolean ALL_SUCCESSFUL_SO_FAR = true
    for w in 1 to n^2:
      A = Adjacency_matrix(G)
      A[v][v] += w
      B = Adjacency_matrix(H)
      B[u][u] += w
      if not Isospectral(A,B)
        ALL_SUCCESSFUL_SO_FAR = false
        break;
    if ALL_SUCCESSFUL_SO_FAR
      result =  Graph_Isomorphism(G\v,H\u)
      if result is not NULL :
        return result.append( (v,u) )
      else :
        return NULL
  return NULL     
```
## Conditional Correctness:
If we assume that given G and H are isospectral, but not isomorphic, 
and the probability that a reweighting breaks their cospectral link is 1/n.
Then with probability 1-1/n the link is not broken,
so we need to run many O(n) times to get a constant probability of breaking it, 
and we then run O(n) of those runs to guarantee breaking cospectrality with exponential probability.
## Runtime:
The worst case runtime is O(n^4*I(n)) where I(n) is the time to run an isospectral test on a n by n matrix pair.
We could optimize this further if we had more data on how rare isospectralness was on non-isomorphic matricies.
I believe Isospectralness can be tested in O(n^3) time by computing all the eigenvalues or the characteristic polynomial.
Whichever is more numerically stable.

# General Idea:
This reduces graph isomorphism to a heuristic of isospectralness on a weighting of the graph.
## Open questions:
Might we only need 1 or 2 weightings of the graph?
since the characteristic polynomial of the weight as a varaiable is degree 1?

When G and H are isospectral but not isomorphic,
is it possible for all v in V(G) , ther exists a u in H, G\v is cospectral with H\u ?
That might construct a counter example to this algorithm.

