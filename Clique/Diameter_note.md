## Introduction
I keep returning to these ideas of the start of a clique algorithm.
However they survive as reductions not complete algorithms.
Here is the complete sensible idea:
# A QuasiPolynomial Reduction From General Clique Problem to Clique Problem with Diameter 2 
Whenever there is a pair of verticies u , v such that the distance in G between them is at least 3,
then the neighborhood of u N(u) does not intersect the neighborhood of v N(v).
Thus ``` |N(u)| + |N(v)| <= n ```  and therefore one of these neighborhoods is less than ```n/2```
If we recurse on the smaller neighborhoods, using the inclusion exclusion principle.
Then while the diameter is at least 3, 
the recursion relation is ```T(n) <= T(n-1) + T(n/2)``` 
This leads to a quasipolynomial time like slowsort, but for an actually interesting problem.
# A QuasiPolynomial Reduction from General Clique Problem to Clique Problem with "Few" P_4s
Given a graph G, either it has a path on four nodes (a P_4) or it doesn't.
If G is P_4 free, fast algorithms for clique on G exist.
A Gem is a P_4 with an additional vertex connected to the other 4 verticies.
If G has a P_4 which induce less then (n-4)/2 gems in G, 
then one of the verticies of that P_4 has a neighborhood of size less than ```7/8n```
So similar to before the recursion relation is ```T(n) <= T(n-1) + T(7n/8)```
And we can quasipolynomial reduce until:
All P_4's in G induce at least (n-4)/2 gems in G.
But if we follow a counting argument ```n choose 5 >= #Gems >= #P_4(n-4)/2```
Then  with some algebra```#P_4 <= n choose 4 * 2/5```
I'm not sure that counts as few but its neat.
## Conclusion
I'm interested in if there is a class of graphs defined by induced subgraphs,
where the clique problem can be solved in quasipolynomial time,
but we don't know how to solve it in polynomial time.
