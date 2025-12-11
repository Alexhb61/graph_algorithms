# Introduction
This note is an attempt to prove that NP = coNP
by showing that there is a decomposition for graphs without majority cliques ( 2w(G) <= n(G) ) that ALWAYS exists.
We first introduce the terms of the relevant graph theory.
Then we show that the decomposition only exists for graphs without majority cliques.
Finally, we write out the nondeterministic algorithm for discovering such a decomposition.
(It uses secret knowledge in one step, and so isn't a PvsNP attempt).

# Preliminaries:
## Graph operations:
We will need a common graph operation: 
#### Graph Join
We can specify it in the following way:
Take 2 graphs A, B put them next to one another,
and connect all verticies from one graph to the other.
Do not touch the internals of A or B.

#### Lemma for cliques with graph join.
w(G join H) = w(G) + w(H) 
The proof of this fact is very clear,
but might be more clear for some readers in the complement.
Remember that w(J) is the size of the maximum clique of J.
Also that complete graphs add by joins K_a Join K_b = K_(a+b) .

In order to discuss the decomposition we will need a more complicated operation on 3 graphs.
I don't know if this is already in the literature somewhere.
#### Partial Join
A undirected graph A,C and a bipartite graph B. 
Let B have as many left verticies as A has verticies,
and as many right verticies as C has verticies.
Then we can note this A +(B)+ C.
I would make up a fancy notation if I had the resources but anyway.

#### Partial Join Lemma:
A disjoint_union C  is a subgraph of A +(B)+ C .
A +(B)+ C is a subgraph of A join B .
This should be clear from definitons.

## Common graphs:
All graphs are simple and finite.
#### 2K1
The graph of 2 verticies and no edges.

#### Claw
The graph of 4 verticies and 3 edges formed in the shape of a claw.
It has 3 verticies of degree 1, and 1 vertex of degree 3.

#### Odd antihole + u
Take a cycle graph on an odd number of verticies.
Take its complement (swap out edges for antiedges and vice versa)
Then add a universal vertex u which is connected to all the verticies.
I don't have a good name for these, they aren't quite co-wheels.

#### Berge Graph:
A graph with no odd hole 
(induced cycle on an odd number of verticies of at least 5 verticies)
and no odd anti-hole 
(induced co-cylce on an odd number of verticies of at least 5 verticies).

## Big Theorem:
Berge Graphs are exactly the perfect graphs.
Graphs where the maximum clique is equal in size to the minimum coloring
(and this property is true on all induced subgraphs).
This theorem is huge and will be the big hurdle to conquer in order to formalize all this.

