# Introduction
While studying topological graph theory, I found the generalization of whitney's planarity criteria to be interesting.
This criteria is that cycles in the graph always correspond to topological cuts in the topological dual. 
However, a single topological cut may or may not correspond to a graph cut in nonplanar (non zero genus) graphs.
This note is focused on a far simpler proof that SL = L  which uses this interesting fact.
# Lemma:
The subroutine of taking the topological dual is clearly in logspace
because of Cook-Mckenzie's paper on L-complete problems
which shows the L-completeness of taking the topological dual of a tree. 
A task which is no harder in a more connected graph.
## Remark:
It is unknown how to do it in a lower complexity class, because the main method of converting a permutation from pointwise form to cycle form is pointer doubling.

# Undirected Connectivity
The classic problem for SL which can be solved in reasonable work and logspace.

## Algorithm:
Pseudocode for the algorithm.
```
UPATH(G,u,v):
  if (u,v) is an edge in G:
    return true
  G' = G add (u,v) labelled uv
  H = TopologicalDual(G')
  return not is_loop(H,uv)
```
## Work/Space :
By the lemma, the topological dual subroutine is in logspace.
The subroutine of checking whether an edge has become a loop is clearly in L.
Both subroutines should take nearly linear work.

## Correctness:
If u and v are 1-connected in G, then after adding the edge uv they are two connected.
Then uv participates in a cycle in G'.
Then by topological duality, uv participates in a topological cut in H.
Thus uv can not be a loop, as no loop participates in any cut.
If u and v are disconnected in G, then the edge uv forms a bridge in G'.
Then by topological duality, uv participates in a cycle set of size 1 in H.
Thus, uv is a loop in H.

# Bipartiteness:
This problem is logspace equivalent to undirected connectivity, but is still interesting from a graph theory perspective to consider separately.

## Algorithm:
```
Is_Bipartite(G):
  H = Topological_Dual(G)
  for v in V(H):
    if degree(v) % 2 == 1 :
      return false
  return true    
```
## Work/Space:
By the lemma, taking the topological dual takes logspace.
If H is represented as an edge list,
we need 1 small number to track the vertex,
and can count the degree mod 2 with 1 bit 
while going through the list on the tape.
Thus this takes logspace.
And clearly nearly linear work.

## Correctness:
If G is bipartite, then G has only even cycles.
Then H has only even degrees because all verticies of H correspond to some cycle of G.
This even degree condition is equivalent to H being eulerian.
If H is eulerian, then H can be decomposed into cycles.
??? MISSING STEP???

# Conclusion:
This note seems correct, but not of high impact or importance.
Thus it will remain a note for now.
