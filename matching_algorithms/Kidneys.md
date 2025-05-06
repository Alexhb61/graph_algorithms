# Introduction
This note is an attempt to review the literature of kidney transplants to see 
if they have already considered the formulation of a maximal (or approximate maximum) hypergraph matching.
This wikipedia article was organized after I was 1st researching this in 2021:
https://en.wikipedia.org/wiki/Optimal_kidney_exchange#cite_note-7
Yep, the 2009 paper definitely mentions hypergraph matching in the middle of its reduction chain.
Furthermore, it presents a nontrivial approximation algorithm.
However, the paper seems to predate the research direction of high degree hypergraphs.
https://en.wikipedia.org/wiki/Perfect_matching_in_high-degree_hypergraphs
https://en.wikipedia.org/wiki/Hall-type_theorems_for_hypergraphs
https://en.wikipedia.org/wiki/3-dimensional_matching
Which may or may not be of interest to the kidney transplant problem.

# Thought 1: Reduce Problem to high codegree hypergraphs
The kidney Transplant Problem is both high degree and weighted (when focusing on short cycles),
and so might not be able to use the high degree algorithms directly. 
The problem is also not an uniform hypergraph.
However, in a similar way to maximum matching is reduced to perfect matching by adding dummy nodes we
should be able to add dummy patients of varying amount to extend the smaller edges(edges on <k patients)
into full edges (for whatever k we choose).
Likewise, for each set of k-1 "patients" we can do a single simultaneous binary search on
the codegree of the graph, and include the best (1/k+y)n edges for y searched and greater than zero.
The real question I would need expertise on is what actually is the codegree of the problem.
Is it high enough for this reduction to work?
#### More precisely is the codegree > n/k or < n/k ?
#### If we fix k-1 patients, how many different patients could complete that cycle?
#### This second question makes an answer of no (the codegree is low) much clearer.
We can't exactly increase k because both k shows up in the exponent of the algorithm's runtime
and k shows up as the logistical bound of feasibility.

# Thought 2: Use maximal matchings to augment existing matchings.
Given a current matching ```M``` in a hypergraph.
If we find any matching```N``` in a hypergraph, (say a maximal matching)
We might find a better matching by looking at the set difference of ```M``` and ```N``` :
 ```M\N U N\M``` Treating this as a bipartite graph (by swapping the roles of verticies and edges)
and finding its maximum (weight) independent set ```S``` in polynomial time (because its bipartite).
This would then let us augment the matching to ```M/\N U S ``` . (where we keep the intersection of ```M``` and ```N```)
This heuristic algorithm doesn't have a clear quality bound from my perspective.
But it seems simple to implement, and easy to work with because we can stop it midway.

# Thought 3: Starting Kidney Transplant Chains with Deceased Donors.
While the time between knowing a person is dead and donating their organs might be too short to run transplant chain computations during,
we can prepare for a potential good match, by looking for the best chain starting with any particular patient.
Then when a deceased donor shows up, we need only compare that donor to patients and pick the best chain created.
Even if we want to treat all patients as a ranked priority list,
we can still search for chains of maximum weight where patient of priority ```i``` has a weight of ```2^(-i)```.
Even if we want to compare chains based on their weakest link,
We can still search for chains of minimum weight where each transplant of quality integer ```w``` has a "weight" of ```k^-w``` (where k is one more than the maximum length of any chain). 
This will act as picking a "best" chain sorted by minimum link weight.
#### What metrics can be used to compare transplant chains?
Is this kind of work already being done? Maybe.

# Thought 4: Kidney vs Liver Donation
These two donation structures seem roughly equivalent mathematically.
Why do they have different start time in procedures in the US?
https://www.amjtransplant.org/article/S1600-6135(22)08512-4/fulltext

# Thought 5: Reduce Problem to High 1-degree HyperGraphs.
The paper linked below shows sufficient conditions for 
different degrees of a hypergraph to have a perfect matching.
https://epubs.siam.org/doi/10.1137/080729657
Even if the proof is non-constructive,
Given a hypergraph with a perfect matching.
If we pick the highest weight edge where the remaining graph has a perfect matching,
we should get a perfect matching of fairly high weight.
This will take O(nm) time on a n vertex, m hyperedge graph.
Does this procedure give a maximum or approximately maximum weight? I doubt it.
There might be reductions from maximum weight perfect matching to perfect matching,
but I doubt they preserve high degree (I know of some on simple graphs, but not hypergraphs).
#### Does the Optimal Kidney Exchange problem have sufficiently high 1-degree in practice?

# Thought 6: Kidney AND Liver Chain
I originally had this idea in a different organ setting.
What if the patients who needed a kidney (part of a liver), donated part of their liver (one of their kidneys)?
#### I am not qualified to judge the feasibility of this from a recovery standpoint.
But from a consent standpoint, I think this is better!
There is no strong social bond required.
The trade from each patients perspective is clear.
And Computationally, I believe this makes the problem entirely feasible.
#### This Formulation is not NP-hard.
To the best of my understanding, being able to donate a body part to someone is an acyclic relation.
#### Immunology Question: Are there any loops in donation compatibility relation other than perfect matches?
Obviously, each perfect match can either be turned into a 2 person trade or a single node in the network.
Then the network of who can donate to who is acyclic, 
and we can apply dynamic programming techniques to find the highest weight paths of bounded length.
#### Ethics Question: Can the kidney score and liver score be added in any reasonable sense?
```
//given an acyclic donation graph G, and a max path length k
Solve(G,k)
Let V = Topologically_Sort_Verticies(G)
Let Best_Path = Array[V][k]
For each v in V:
  // This inner loop might be parallelized as
  For each l in 1 to k:
    if l == 1:
      Best_Path[v][l] = 0
    if l > 1:
      Best_Path[v][l] = -infinity
      For each u in OutNeighborHood(G,v)
        Best_Path[v][l] = max(Best_Path[v][l], G.weight(u,v) + Best_Path[u][l-1] )
Best_Global_Path_Weight = 0
For each v in V:
  For each l in 1 to k:
     Best_Global_Path_weight =  max( Best_Global_Path_Weight, Best_Path[v][l] )
return Best_Global_Path_weight
```
Here is a correct dynamic programming algorithm for the problem when path length needs to be bounded.
It will need slight modification to keep track of what the path is in addition to its weight.
   
    
    
