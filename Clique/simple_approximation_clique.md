# Introduction 
For any epsilon > 0 , Can the clique problem be solved in 2^epsilon*n time?

I am interested in this algorithmic question and also approximating it.
These approximations might not be of computational complexity importance.

After reading more on the Erdos-Hajnal conjecture, I came across the "three-fourths" proven version:
### 3/4 Erdos Hajnal
If a graph G is H-free, then G has either a complement of a biclique or a clique.
Note: there is no specification on the edges on the sides of the biclique.

# Reduction sequence:
Clique in General Graphs can be reduced in many ways to 
clique in graphs with some family of excluded induced small graphs. 
Then, 
Clique in graphs with some family of excluded induced small graphs
can be reduced to clique in graphs with a polynomially large clique in every induced subgraph.
But
Can clique with a polynomially large clique in every induced subgraph be solved quickly?
## Excluding small graphs in arbitrary exponential time:
This can be done in many ways, but the simplest way is to exclude a path:
```
Clique (G):
  find a Path of size k P in G
  if P was not found:
    inner_Clique_algorithm(G)
  C = 0
  For each edge uv in P :
    C = max ( C, Clique(G \ P + u,v )
  return C
```
As k increases this will have a lower and lower exponent in its runtime
as seen by its recurrence relation.
``` T(n) <= (k-1) T(n-k+2) + n^k ```
## Using the 3/4 Erdos-Hajnal theorem:
This can be done in a few ways, but the simple one is this:
```
Clique(G) :
  find a subgraph AuB of 2K_k which is an induced graph in G :
  if AuB was not found:
    inner_clique_algorithm(G)
  return max( clique(G \A), clique(G\B) )
```
As k increases this will have a lower and lower exponent in its runtime
as seen by its recurrence relation.
```T(n) <= 2 T(n-k) + n^2k ```
## Missing section
How do you solve the final problem faster than usual? I don't know.

# Conclusion
I don't know if this is interesting, but nonetheless it is a note I finished.
