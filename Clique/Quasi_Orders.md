# Introduction
Here are some notes on quasi-orders.
I am interested in their relation to computational complexity via graph theory.

## A relationship between finite obstruction sets and NP vs coNP
1. Let P be an unlabelled graph Property (i.e. it maps Graphs to true or false and is equivalent on isomorphic graphs)
2. Let S be a size function (i.e. it maps Graphs to the natural numbers)
3. Let O be a finite set of (potentially failable) operations which map graphs to graphs (or fail) and can be computed(and thus described) in polynomial time.
4. Let T be a finite set of finite graphs.
#### If 
1. P is NP-complete to compute on simple finite graphs.
2. The operations O decrease the size S of the all graphs ( for all g in Graphs: for all o in O : s(g) > s(o(g)) )
3. P is closed under the operations O ( for all g in Graphs : for all o in O : P(G) -> P(o(G)) )
4. The size function is polynomial bounded in terms of vertex count (there exists a polynomial p : for all g in Graphs : s(g) < p(n(g)) )
5. T is a finite obstruction set for P under O. ( for all g in graphs\T : not P(g) -> there exists o in O:  not P(o(g)) and o(g) succeeds ) ( for all graphs j in T:  not P(j) )
#### Then NP=coNP
Proof: 
#### lemma 1: The certificate of an operation sequence {o} followed by a terminal graph T is evidence that not P(G)
If the graph G after applying the sequence of operations {o} is isomorphic to T, then the certificate works.
We know this because by property 5, not P(T). and by property 3 not P(o(h)) -> not P(h) thus the chain of operations demonstrates not P(G) from the bottom up.
#### lemma 2: This certificate is polynomial in length
proof by induction on graph size: |C| <= s(G) < p(n(G))
Base case: g is isomorphic to a terminal graph and thus has certificate length 1.
Inductive case: g has an operation o that can be performed while maintaining not P(G).
the certificate for o(g) is length s(o(g)) which is < s(g) by porperty 2, and we can form a certificate for g
by concatenating the operation o to the certificate for o(g) .thus |C| <= 1 + s(o(g)) <= s(g). Thus the claim holds.
#### lemma 3: The certificate can be checked in polynomial time.
Each operation can be performed in polynomial time by definition 3,
so the whole certificate takes polynomial time to check until you reach the terminal graph 
whose isomorphism test takes constant time to check because there are only finitely many graphs in T.
#### combining lemmas:
Thus, given all 9 assumptions, we have shown that there is a Polynomial time certificate for a coNP hard problem. The conclusion follows.
### Concern
I get too excited/distracted by potential instatiations of this theorem.
## Problem with the canonical antichain for subgraphs:
To the best of my current understanding, a property closed under subgraphs only has a finite obstruction set if and only if it has finite interesection with the canonical antichain.
Said canonical antichain is the cycle on n verticies and the path on n verticies with doubled endpoints.
If that understanding holds, I have a simple counter-example.
#### The property of graphs with maximum degree = k for k>3 has a single forbidden subgraph and infinite intersection with the canonical antichain for subgraphs.
It should be clear that if a graph has maximum degree >= k +1 then it has the star on K+2 nodes as a subgraph.
Likewise if the maximum degree of a graph is k it lacks the star on k+2 nodes as a subgraph.
However, since the two antichains have maximum degree 3, they completely intersect this property. Thus an error has occurred somewhere.
## worked example for 2-colorability (not NP-complete)
It is well established that the chromatic number is monotonically decreasing under edge deletion an vertex removal.
```X(g) >= X(g-uv) and X(g) >= X(g\v) ```.
It is well known that the obstruction set for 2-colorability are the odd cycles.





