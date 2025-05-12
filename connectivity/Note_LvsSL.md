# Introduction
While studying topological graph theory, I found the generalization of whitney's planarity criteria to be interesting.
This criteria is that cycles in the graph always correspond to topological cuts in the topological dual. 
However, a single topological cut may or may not correspond to a graph cut in nonplanar (non zero genus) graphs.
This note is focused on a far simpler proof that SL = L  which uses this interesting fact.

# Algorithm:
Pseudocode for the algorithm.
```
UPATH(G,u,v):
  if (u,v) is an edge in G:
    return true
  G' = G add (u,v) labelled uv
  H = TopologicalDual(G')
  return not is_loop(H,uv)
```
# Work :
The subroutine of taking the topological dual is clearly in logspace
because of Cook-Mckenzie's paper on L-complete problems
which shows the L-completeness of taking the topological dual of a tree. 
A task which is no harder in a more connected graph.
The subroutine of checking whether an edge has become a loop is clearly in L.

# Correctness:
If u and v are 1-connected in G, then after adding the edge uv they are two connected.
Then uv participates in a cycle in G'.
Then by topological duality, uv participates in a topological cut in H.
Thus uv can not be a loop, as no loop participates in any cut.
If u and v are disconnected in G, then the edge uv forms a bridge in G'.
Then by topological duality, uv participates in a cycle set of size 1 in H.
Thus, uv is a loop in H.

# Conclusion:
This note seems correct, but not of high impact or importance.
Thus it will remain a note for now.
