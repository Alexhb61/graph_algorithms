# Arbitrarily fast exponential reductions
## Introduction
I don't believe the Exponential Time Hypothesis.
I do not have any complete proofs of that belief.
However, below are some arbitrarily fast exponential time reductions from the general clique problem to weird instances.
## Reductions from the general clique problem to clique problem with bounded independence number.
This is a simple enough proof that it might already exist in the literature, but I don't think I've seen it.
The basic key fact about clique's and independent sets is they have maximum intersection size 1.
#### Claim: for all epsilon > 0, there exists an a > 0, In O*(2^(n*epsilon)) time
#### Clique on G can be reduced to at most 2^(n*epsilon) clique problems on induced subgraphs of G
#### where each subgraph of G has independence number < a
Proof: Let a be the smallest integer such that epsilon > lg(a+1)/a
Then Given an instance of clique with independence number >= a
We first find an independent set S of size a in polynomial time.
Then we solve clique on the a problems induced by the neighborhood of any one vertex of S (adding 1 to the answer we get from that).
And also solving clique on the problem induced by removing S from G.
This simple algorithm's recursion relation is:
``` T(n) <= (a+1)T(n-a) + O(n^a) ```
Which results in a recursion tree of size at most 2^(epsilon*n)
and base cases wherever the independence number drops too low.
## Reductions from general clique to clique on a graph with many forbidden induced subgraphs.
#### Claim: For all espilon > 0, In O*(2^(epsilon*n)) time
#### Clique on G can be reduced to at most 2^(epsilon*n) clique problems on H-free induced subgraphs of G
#### where H is a family of graphs and for all h in H, h has < poly(1/epsilon) verticies .
#### and H contains >= 2^poly(1/epsilon) many graphs
I have two different algorithms with this structured behaviour.
They can both be seen as generalizations of the above algorithm with independent sets.
### Algorithm 1
There exist parameters h,a to be chosen in the analysis section.
The trivial clique is the empty set.
```
Clique(G) :
Find J an induced subgraph of G of vertex size h with maximum clique of size <=a.
if J is found:
  w = clique(G\V(J) )
  for each nontrivial clique C of J :
    w = max(w,clique(G[N(C)]) + |C|)
  return w
return H_Free_clique(G)
```
The Recursion relation is ```T(n) <= O(n^h(h^(a+1))) + O((h+a)^a) T(n-h)
We can look at n^h many possible subgraphs J and do h^(a+1) work to confirm the clique bound.
The number of subproblems is at most h + a choose a divided by a! since we are choosing all cliques of size at most a
and each subproblem removes the whole graph J so its size at most T(n-h)
### Choosing h, a
we need epsilon > log(subproblem_count) / (chip_size) in order for the claim to hold
but we have two parameters h,a and let h = theta( a^2/loga) 
so that even if the independence number of a graph becomes 2,
we can still find a J with the desired h,a in theory
This make a of order 1/epsilon with some log(1/epsilon) factors.
It also make h of order 1/epsilon^2 with some log factors.
So that means the graphs excluded when we fail to find such an H have the correct size.
### H family is large for algorithm 1
Good REFERENCE OR Strong PROOF NEEDED
Weak Argument:
If H contains less than 2^(1/epsilon) graphs, and there is constructive proof of that.
Then a faster circuit for root n clique is to just go through that list of graphs and check isomorphism.
