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
I have two different reductions with this structured behaviour.
They can both be seen as generalizations of the above reduction with independent sets.
### Reduction 1
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
By N(C) we mean the set of verticies adjacent to all members of C but not C itself.
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
### H family is large for Reduction 1
Good REFERENCE OR Strong PROOF NEEDED

Weak Argument:
If H contains less than 2^(1/epsilon) graphs, and there is constructive proof of that.
Then a faster circuit for root n clique is to just go through that list of graphs and check isomorphism.

### Reduction 2
let p be the smallest integer such that epsilon > lg(2p+1)/p
I'm going to use pathwidth even though treewidth might be better because I'm more sure it works.
```
Clique(G):
Find an induced subgraph J of G which has 2p verticies and pathwidth at most p-1
if found J:
  w = Clique(G\V(J) )
  for each node n in path decomposition of J :
    w = max( w, Clique( G[N[n]] ) )
  return w
return H_Free_clique(G)
```
Note that we are including all the verticies of a node in its subproblem.
By N[n] We mean all the verticies adjacent to any vertex inside the node n includeing all the verticies inside n.
I am under the impression that a path decomposition has as many nodes as the underlying graph has verticies.
Because each clique in the small graph is contained by one node, this considers all possible cliques.
The above reduction has recursion relation:
```T(n) <= poly(n) + 2p T(n-p) + T(n-2p)```
because it considers at most 2p nodes each excluding the p verticies not in the node. And one subproblem without the whole graph.
Clearly, the graphs excluded are in poly(1/epsilon)
### Reduction 2 searches through a large family
If we put 2p verticies on the number line at unit intervals, and let them connect to any verticies at distance at most p-1 away,
this constructs a graph of pathwidth p-1 with at most 2p(p-1) edges.
There are 2^(2p(p-1)) labelled graphs constructed this way each have at most (2p)! many automorphisms.
So the number of unlabeled graphs is at least 2^(2p(p-1)-2plogp) which is still in 2^poly(1/epsilion)

## Reductions from General Clique to Clique on Graphs of Treewidth n-O(1)
### tangent about treewidth decompositions
##### Proposition 1: A n vertex graph of treewidth at most k has representation as a partial k-tree where the k-tree has n-k maximal cliques
Proof: Start with a k-tree with one node and keep adding nodes/verticies the numbers of nodes and verticies increase in lockstep.
##### Proposition 2: A n vertex graph of treewidth at most n-k has representation as a partial n-k-tree where the n-k-tree has at least n-2k+2 verticies common to all nodes and at most n-k verticies (assuming the graph is not the complete graph on n verticies)
Proof: Start with a n-k-tree with two nodes it has n-k common verticies. As we add the k-2 verticies to make a n vertex graph, each addition might remove one common vertex or remove zero common verticies.
#### Theorem: For every fixed k, Given an input graph G on n verticies, in polynomial time O(n^(2k-2)), we can find a n-k tree T such that G is a spanning subgraph of T.
Proof: We choose between 2k-2 and k active verticies c for the decomposition (as opposed to the other verticies which will be common to all nodes).
Then we find a tree decomposition of width |c|-k on those c verticies which can be computed in time exponential in |c| which is dominated by the number of choices of active verticies. Note we are assuming n >> k.

Remark: This also acts as forbidden induced subgraph characterization of n-k treewidth graphs.
### Reduction
#### Theorem:  for every epsilon > 0 we can reduce clique on a graph G to clique on graphs H which have treewidth n(H)-poly(1/epsilon)
#### In O*(2^(epsilon*n)) time with a 2^(epsilon *n) of subproblems
let p be the smallest integer such that epsilon> lg(p)/(p-1)
Proof: 
```
Clique(G) :
(found, tree_decomposition) = High_tree_decompostion(G,p) # Finds an n-p treewidth decomposition if it exists
if found:
  w = empty_clique()
  for each node in tree_decomposition:
    w = max(w, Clique(G[node]))
  return w
return Nearly_n_Treewidth_clique(G)
```
Given the theorem above and the proposition above this algorithm has a recursion relation:
```T(n) <= pT(n-p+1) + O(n^(2p-2)) ```
Which by the chip and be conquered master theorem takes 2^(epsilon*n) time / subproblems.

## Conclusion
So we can reduce the clique problem on general graphs, to the clique problem on graphs with a lot of excluded graphs.
So what? Well To my understanding most random graphs of sufficient size, are universal (see rado graph).
And so all the base cases of these reductions on a random graph are small. 
Does this hold for interesting but not random cases? I don't know, but I think so. 
I think the treewidth version might subsume the pathwidth version.
Especially since I have a characterization of the terminal state.

