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
Take 2 graphs A & B put them next to one another,
and connect all verticies from one graph to the other.
Do not touch the internals of A nor B.

#### Lemma for cliques with graph join.
w(G join H) = w(G) + w(H) 
The proof of this fact is very clear,
but might be more clear for some readers in the complement.
Remember that w(J) is the size of the maximum clique of J.
Also that complete graphs add by joins K_a Join K_b = K_(a+b) .

In order to discuss the decomposition we will need a more complicated operation on 3 graphs.
I don't know if this is already in the literature somewhere.

#### Partial Join
A undirected graph A,C and a bipartite undirected graph B. 
Let B have as many left verticies as A has verticies,
and as many right verticies as C has verticies.
Then we can note this A +(B)+ C.
I would make up a fancy notation if I had the resources but anyway.

#### Partial Join Lemma:
A disjoint_union C  is a subgraph of A +(B)+ C .
A +(B)+ C is a subgraph of A join C .
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
(induced co-cycle on an odd number of verticies of at least 5 verticies).

## Useful Theorems:
#### Strong Perfect Graph Theorem
Berge Graphs are exactly the perfect graphs.
Graphs where the maximum clique is equal in size to the minimum coloring
(and this property is true on all induced subgraphs).
This theorem is huge and will be the big hurdle to conquer in order to formalize all this.

#### Other Big theorem: maximum clique on perfect graphs is in P.
This fact would be needed if you wanted to run the nondeterministic algorithm
It involves a lot of theoretical machinery.
It is not as necessary, because you could just provide a coloring 
in addition to the graph in order to show that the clique size is bounded.

#### Tiny lemma: 
If G is a subgraph of H,
Then w(G) <= w(H). 
Proof follows from deleting edges only making to get from H to G,
only can decrease the clique number.

# The Decomposition Certificate.
The decomposition is simply the partial joins of the 4 mentioned graphs:
2K1, Claw, Odd Antihole + u, Berge Graph.
Where the Berge graphs in question also have no majority clique.
The lack of a majority clique in a graph with this decomposition can be shown by
the lack of the majority clique in each of the specified graphs, 
and combined with the lemmas mentioned earlier. 
For example as ((A +(B)+ C) +(D)+ E) +(F)+ G 
is a subgraph of A join C join E join G .
So w(((A +(B)+ C) +(D)+ E) +(F)+ G) 
<= w(A join C join E join G ) 
= w(A) + w(C) + w(E) + w(G) 
<= |A|/2 + |C|/2 + |E|/2 + |G|/2 
<= n(((A +(B)+ C) +(D)+ E) +(F)+ G)/2

We can see by inspection that the 2K1, and Claw have maximum clique at most half.
We can see that the Odd Antihole + u also has maximum clique at most half
since an odd antihole of size 2c+1 has maximum clique size c.
Lastly, the Berge Graphs are perfect,
and so we can show they have a maximum clique of size at most n/2 
by providing a coloring with n/2 colors.

# The existance Proof:
We claim that for any finite simple graph on 2k verticies,
if it has no maximum clique larger than k,
then a decomposition of the form mentioned earlier exists.
The proof is by induction on the size of the graph.
The base cases are each of the 4 kinds of graphs that show up in the decomposition.
## Inductive Case
The inductive case is below (presented as a recursive algorithm with secret knowledge).
1. Given G on 2k verticies with maximum clique size k.
2. Let C be the maximum intersection of any 2 maximum cliques of G (or be the unique maximum clique of G).
3. If C is the empty set then (by konig's theorem) the complement of G has a perfect matching, and we can decrease G's size by separating out one 2K1 graph into the decomposition while perseving the next G having a perfect matching.
4. else if C has a vertex v whose degree is not 2k-1 in G then we can separate out a 2K1 in the form v and a non-neighbor.
5. else if C has a universal vertex u, and G has a cotriangle C (an independent set of size 3) we can separate out u+C the claw.
6. else if C has a universal vertex u, and G has an odd-antihole O We can separate out O+u the odd antihole + u.
7. else we know there is no odd antiholes and no odd holes ( which would contain large independent sets), so the graph is Berge and our decomposition is done. We can report its coloring as the end of our certificate.
### Expanding on the mention of Konig's theorem:
If C is the empty set, then all pairs of maximum cliques have no intersection.
Then we can view that pair of cliques as the sides of a co-bipartite graph.
Then since this graph has no clique of size k+1 or larger, 
we know that its complement G' has no independent set of size k+1 or larger.
And we know G' has no vertex cover of size  k-1 (or smaller)
then by Konig's theorem we know G' has a maximum matching of size k.
(a perfect matching).
### NOTE nondeteministic knowledge:
In order to run this algorithm and produce a specific decomposition,
one would need to know C or something related, especially for step 4
where which vertex to choose is dependent on C. 
This is what makes this a nondeterministic algorithm.
In step 3, we could just always check for a perfect matching.
In step 5 and 6, we could just always use some universal vertex.

# Conclusion.
We have shown the coNP-hard problem of
(Does Graph G on 2n verticies have no clique larger than n?) 
has a NP algorithm. 
Thus showing that coNP = NP.
It is already an exercise for students to show that half-clique is NP-hard.
Thanks for reading.
Hopefully any errors are caught sooner rather than later.

