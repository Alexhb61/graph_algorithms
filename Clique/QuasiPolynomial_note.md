## Introduction
I keep returning to these ideas of the start of a clique algorithm.
However they survive as reductions not complete algorithms.
Here are the complete sensible ideas:
# A QuasiPolynomial Reduction From General Clique Problem to Clique Problem with Diameter 2 
Whenever there is a pair of verticies u , v such that the distance in G between them is at least 3,
then the neighborhood of u N(u) does not intersect the neighborhood of v N(v).
Thus ``` |N(u)| + |N(v)| <= n ```  and therefore one of these neighborhoods is less than ```n/2```
If we recurse on the smaller neighborhoods, using the inclusion exclusion principle.
Then while the diameter is at least 3, 
the recursion relation is ```T(n) <= T(n-1) + T(n/2) + poly(n)``` 
This leads to a quasipolynomial sized recursion tree like slowsort, but for an actually interesting problem.
# A QuasiPolynomial Reduction from General Clique Problem to Clique Problem with "Few" P_4s
Given a graph G, either it has a path on four nodes (a P_4) or it doesn't.
If G is P_4 free, fast algorithms for clique on G exist.
A Gem is a P_4 with an additional vertex connected to the other 4 verticies.
If G has a P_4 which induce less then (n-4)/2 gems in G, 
then one of the verticies of that P_4 has a neighborhood of size less than ```7/8n```
So similar to before the recursion relation is ```T(n) <= T(n-1) + T(7n/8) + poly(n)```
And we can quasipolynomial reduce until:
All P_4's in G induce at least (n-4)/2 gems in G.
But if we follow a counting argument ```n choose 5 >= #Gems >= #P_4(n-4)/2```
Then  with some algebra```#P_4 <= n choose 4 * 2/5```
I'm not sure that counts as few but its neat.
# A QuasiPolynomial Reduction from General Clique Problem to Dense Clique Problem
Let c be fixed and less than 1.
Given a graph G on n verticies, either its minimum degree is at most cn or we might call the graph dense.
While a graph is not dense in this way, 
applying the inclusion-exclusion principle to the minimum degree vertex
gives the recursion relation: ```T(n) <= T(n-1) + T(cn) + poly(n)```
Which is results in a quasipolynomial sized recursion tree whenever c is a constant.
# A QuasiPolynomial Reduction from General Clique to High Pathwidth Clique Problem.
While the minimum pathwidth problem itself is NP-complete,
there are polynomial time approximation algorithms with approximation ratio r = O(log^3/2(n)).
If we define a high pathwidth graph to be when the pathwidth is at least cn/r for some fixed c less than one.
Then Given a graph G and the task of finding the maximum clique,
we can reduce it to finding the clique in quasipolynomially many instances of High pathwidth graphs.
```
CliqueReduction(G):
if number of verticies of G < k :
  return brute_force_clique(G)
approximate_pathwidth, path_decomposition = approximate_pw(G)
current_best_clique = {}
if approximate_pathwidth < cn :
  for each node in path_decomposition :
    w = CliqueReduction(G)
    if len(w) > len(current_best_clique) :
      current_best_clique = w
else :
  return HIGH_Pathwidth_clique(G)
return current_best_clique 
```
The recursion relation is  ```T(n) <= n T(cn) + poly(n) ``` 
Because the approximation algorithm runs in polynomial time, the nonrecursive work is poly(n)
see Feige, Uriel; Hajiaghayi, Mohammadtaghi; Lee, James R. (2005), "Improved approximation algorithms for minimum-weight vertex separators", Proc. 37th ACM Symposium on Theory of Computing (STOC 2005), pp. 563–572, doi:10.1145/1060590.1060674, ISBN 1581139608, S2CID 14097859.
Because the found width of the found path decomposition is at most cn, each subproblem is at most that large.
Because each vertex only ever enters and exits the the path decomposition once, the number of nodes in the decomposition is at most n.
This reduction is quasipolynomial in size because the recursion tree is depth log(n)/log(c) and branching factor n.
Which is roughly equivalent to a branching factor 2 depth O(log^2(n)) tree.
## Conclusion
I'm interested in if there is a class of graphs defined by induced subgraphs,
where the clique problem can be solved in quasipolynomial time,
but we don't know how to solve it in polynomial time.
I'm approaching the problem from trying to find an intersection of properties
where each property can be enforced in subexponential time.
Are there enough properties to form an empty set of graphs? I doubt it.
