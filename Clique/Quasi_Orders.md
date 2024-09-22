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
I found that clique cover is closed under induced minors, and misread a theorem causing me to think it had a finite obstruction set.
When we think of the operations as forming a quasi-order, the quasi-order being well-founded is sufficient for the obstruction set to be finite.
## Problem with the canonical antichain for subgraphs:
To the best of my current understanding, a property closed under subgraphs only has a finite obstruction set if and only if it has finite interesection with the canonical antichain.
For the subgraph relation, the canonical antichain is the cycle on n verticies and the path on n verticies with doubled endpoints.
If that understanding holds, I have a simple counter-example.
#### The property of graphs with maximum degree = k for k>3 has a single forbidden subgraph and infinite intersection with the canonical antichain for subgraphs.
It should be clear that if a graph has maximum degree >= k +1 then it has the star on K+2 nodes as a subgraph.
Likewise if the maximum degree of a graph is k it lacks the star on k+2 nodes as a subgraph.
However, since the two antichains have maximum degree 3, they completely intersect this property. Thus an error has occurred somewhere.
## Worked example for k-colorability
### 2-colorability 
It is well established that the chromatic number is monotonically decreasing under edge deletion an vertex removal.
```X(g) >= X(g-uv) and X(g) >= X(g\v) ```.
It is well known that the obstruction set for 2-colorability are the odd cycles.
However, we can increase the number of operations.
Using Zykov's Recurrence relation ```X(G) = min( X(G +uv), X(G/uv)) where uv not an edge ```, and the knowledge that a triangle is not 2 colorable,
we can create the operation:
Given an induced path on 3 verticies u-v-w inside G:  X(G) <= 2 -> X(G/uw) <= 2 because the triangle occurs in the other side of the recurrence relation.
So 2-colorability is closed under subgraphs and this new operation, but also its only terminal graph is a triangle.
Because 2-colorability is easy to identify, this no certificate isn't very interesting, but it can serve as a warm up.
### 3-colorability
Our 3 operations, are vertex deletion, edge deletion, and partial non-edge contraction.
where we can contract an non-edge uv if adding the edge uv would cause a terminal subgraph to appear.
We can define the set of terminal graphs recursively as follows:
Let H_4 be the set of terminal graphs on 4 verticies (the only graph is the complete graph on four verticies). (it requires 4 colors, and thus is strictly not 3-colorable)
Let H_n be the set of terminal graphs on n verticies.
#### A graph H is in H_n IF:
1. H has n verticies
2. H requires 4 colors.
3. H\v is 3 colorable for all verticies in H
4. H-e is 3 colorable for all edges e in H
5. for all m in [4,n) : for all J in H_m : for all e in edges of J : H is J-e-subgraph-free
#### Key Question is the union of H_n for all n finite? If yes, call that set H_* and note P=NP
The algorithm is as follows:
```
three_colorable(G):
for all h in H_* :
  if h is a subgraph of G :
    return false
for all h in H_* :
  for all uv in edges of h:
    if h-uv is a subgraph of G :
      return three_colorable(G/uv)
return true      
```
Either a terminal graph is reached and the graph requires 4 colors, or a nearly terminal graph is reached and we can reduce the problem to a smaller graph, or neither occurs and by completeness of the terminal set the graph can be 3-colored.
#### Key question if the union of H_n from 4 to n is bounded in quantity as a function f(n)  : then coNP \subset of NP with O(poly(n)f(n)) advice
Given a graph on n verticies whose 3-colorability is desired.
We take our advice to our advice to be the full set of obstructions up to vertex count n.
The certificate is the linear Zykov tree where edge addition sides identify a 4-color requiring subgraph,
and so the tree only extends down the non-edge contraction side until a 4 color requireing subgraph is reached.
This does not fit into P/poly because finding the subgraphs of large size is nontrivial.

Fact: Property 2,3,4 make this a subset of the minimal obstructions under subgraph quasi-order.
Fact: Property 2 and 3 make the minimum degree of H at least 3.
Fact: Property 2 and 3 make the graph H J-subgraph free in a similar way to property 5.

One could extend this to include edge addition as well as non-edge contraction, but then the size function becomes a nontrivial question. (here it is just edges plus verticies)

So the number of operations and terminal graphs keep growing in parallel (if you consider each partial edge contraction a different operation).
## Weird corollaries
if ```NP =\= coNP```, then proving a property is NP-complete is sufficient to prove that it lacks a finite obstruction set under any finite set of P computable operations.
if ```NP =\= coNP```, then all NP-complete properties closed under induced minors have infinite obstruction sets.

