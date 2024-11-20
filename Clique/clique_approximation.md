# Introduction
When stressed, I sometimes make attempts on solving the clique problem quickly.
This attempt builds on a simple extension of the Erdos-Hajnal Conjecture.
The extension would need to hold in order for the algorithm below to approximate.
# Conjectures
### Restating the Erdos-Hajnal Conjecture
For every hereditary family of graphs F, there exists a constant epsilon.
Such that every graph G in F, ```max(clique_number(G),Independent_set_number(G)) >= number_of_verticies(G)^epsilon```
Call this the erdos-hajnal constant for F.
## My Conjecture
For every hereditary family F, the erdos-hajnal constant is easily measured.

Specifically, there exists a single constant c,
for every hereditary family of graphs F,
for any natural number m, 
let ``` d = minimum over all graphs G in F on m verticies( log(max(independent_Set_number(G),clique_number(G)))/log(m) )```
Then the erdos-hajnal constant for F is between ```d-c/m``` and ```d+c/m```
# Adjustable Algorithm
The following algorithm attempts to find the clique number, and when faced with a case it can not handle invokes my conjecture to get an approximation.
## Algorithm
For every epsilon_1 and epsilon_2 greater than zero,
A version runs in 2^(epsilon_1*n) time and 
finds the approximate clique number u to be clique_number^(1-epsilon_2) <= u <= clique_number(G) 
Using the c from the conjecture we first need to find two parameters b, k  where b is an integer.
such that ```1/(k+1) +c/b^(k+1) <= epsilon_2``` and ``` (k+1)lg(2b)/b <= epsilon_1 ```
### Pseudocode
Where N(C) is the neighborhood of C without C, and G[V] is the graph induced by V 
```
clique(G,k,b) :
  if number_of_verticies(G) <= b^(k+1) :
    return brute_force_clique(G)
  Let found_graph = empty()
  for an integer a ranging from 1 to b^k :
    for each subgraph H of G of size ab
      if brute_force_clique(H) <= a
        found_graph = H
        break twice
  if found_graph == empty() :
    return n^(k/(k+1) )
  let w = 0
  for each clique C in found_graph :
    w = max(w, |C| + clique(G[N(C)],k,b) )
  return w
```
## Runtime argument:
we have a subgraph of b^(k+1) which picks at most b^k verticies
T(n) <= (b^(k+1) + b^k choose b^k ) T(n-(b^(k+1) ) )
which can be analyzed via the chip and conquer master theorem.
```log(P)/C <= log(2*b^(k+1) choose b^k ) / b^(k+1)  <= b^k * log(2*b^(k+1) ) / b^(k+1) <=  (k+1)*log(2b)/b <= epsilon_1 ```

## Approximation argument:
When the algorithm fails to find a suitable subgraph, it has ruled out all subgraphs of size b^(k+1) where the clique is at most b^k.
This means that the input graph at that leaf is part of a hereditary family caused by the ruled out graphs.
By the conjecture, the Erdos-Hajnal constant for this graph family is at least ```k/(k+1) - c/b^(k+1)``` 
We get this number by sampling the family at size b^(k+1) ie all the graphs of this size which were not ruled out.
Then our approximation epsilon is merely one mins this expression as desired.
# Conclusion
This isn't quite an algorithm, because its correctness requires a conjecture to be true or at least roughly true, but I think it is solid logic nonetheless.
Furthermore, much work has successfully shown the original conjecture to hold in many cases.
##### personal remark
This is a neat idea, but I don't know if it is paper worthy; regardless, I wanted to write it down so I could move on.
