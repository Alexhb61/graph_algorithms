# Introduction
I am currently interested in mixing strategies for the clique problem.
The unreasonable aim is push against the exponential time hypothesis.
In all practicality, I aim to learn something and practice reinning in my ambition.
The strategy this document aims to explore is mixing local search methods with chip and conquer methods based on specific induced subgraphs.
# Strategy I
## The Graphs
Fix a natural number a,
Let H_a be the family of simple graphs where
for all h in H_a, n(h) =2a and clique_number(h) <= a
## The algorithm
R strength incremental local search:
```
find_clique(G, current_clique)
  for b in 0 to R-1 :
    for b verticies u in current_clique :
      for b + 1 verticies w in V(G) \ current_clique
          C = current_clique - u + w
          if is_clique(G,C) :
            return find_clique(G,C)
  return current_clique
```
## Workload
Each search step when the clique is w sized takes at most the following work:
```(w choose R-1)(n-w choose R) <= n^(2R-1) ```
If we start on the empty clique, the search which increments the clique size each step means we have a total of n^(2R) work.

## Conditional correctness:
#### THM: If G is H_(a+1)-free, then a-strength incremental local search finds the maximum clique
Note 1: it should be clear that n-strength local search works because it is basically brute force.

We will show that if finding the maximum clique requires an (a+1)-strength step ( or stronger step), then it has a graph from H_(a+1).
1. A stronger step with empty starting clique implies a different clique C would need an (a+1) strength step.
2. This (a+1) strength step implies an (a+1) removal and then (a+1) addition step then an (1) addition step.
3. The verticies removed and added in the first two steps imply a graph on 2a+2 verticies.
4. This implied graph has max clique (a+1) because if it had an (a+2) clique there would be a corresponding a-strength step would work for C which fails by premise of being required.
5. Thus we have found a member of H_(a+1) and contradicted the starting premise.
## The Remaining issue
It is unclear to me how to find an interesting chip and conquer strategy for this graph class H_a
because the cliques are possibly half the size of the subgraph. 
That feels big enough that simple strategies don't seem to work.
The remainder of this document is going to attempt to address that.

# Strategy II
## The Graphs
Let the family of simple graphs J_a be the graphs which are subgraphs of 2K_a (the graph with two isolated cliques of size a).
Let the family of simple graphs S_a,b be the graphs s where n(s) = a*b and clique_number(s) <= a .
## The algorithm
The algorithm has three parameters: strength r, search buffer b , descent limit d . 
Roughly Given a current best clique C
Search for a new local maximum while descending no more than d verticies below the height C.
Taking steps which remove c verticies and add back a verticies and where min(a,c) < r
And stop if the number of explored verticies for this local maximum exceed (|C|+a)*(b-2)
#### Known ambiguity: How to make sure each step increases explored verticies.
## Workload:
This is b*n^(2r+d+1) which is a lot.
## Connection between graphs and parameters
#### If G is J_d-free, d-1 step descent is sufficient
d step descent induces a member of J_d
#### If G is S_a,b-free, search buffer b-1 is sufficient, and (a-1)-strength is sufficient.
If the search buffer is exceeded, either search repetition has occurred or a member of S_a,b has occurred.
Because by a counting argument at least a verticies have been replaced b-1 times and those form a graph with clique size at most a, and ba verticies.
### Is search repetition necessary? sometimes? never?
I need to return to this when I'm not tired.
## The chip and conquer Behaviour.
J_a can be broken into two subproblems of chip size a, because no clique bridges the edgeless gap.
S_a,a can be broken into <(a^2 +a choose a) problems of chip size a^2, by just choosing all the cliques in the small graph.
As a increases, the epsilon grows smaller in both of these cases.
### combine
The two techniques might combine to form a sub c^n algorithm for clique.
I am uncertain if the combine or if they fall apart.
# Conclusion
I have presented one slightly rough algorithm which sometimes works, 
and one very rough algorithm which is exciting.
I might return to this document, but I might not.
