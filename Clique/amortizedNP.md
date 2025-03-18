# Introduction
In computational complexity theory, one often attempts to solve a decision problem as an isolated algorithm.
However, when an algorithm is used as a subroutine in some other problem we might want to 
account out its startup costs or rare costs through amortized analysis. After thinking about the
sparsification lemma for k-SAT, I think there are ways to do that.

# Basic Idea:
We can solve a hard problem that is big in description size by reducing it to a problem which is small in size 
and then looking up the answer in a database (and computing the answer if the problem is new to the database).
Note, in order to get a non-uniform non-amortized speed-up, one would need the database to be sufficiently small.

# k-SAT
We can construct this reduction-database pair for k-SAT by using the sparsification lemma 
which in 2^(epsilon*n) time reduces k-SAT on n variables to instances of c-occurrences n-variables k-SAT.
The database's size is at most O(n^(cn) ) because each occurrence has n options.
Downside to using this technique is that the worst case work is 2^(s_k +epsilon)n.
The more notable downside is that the best c for this reduction is exponential in k. (see paper with width reductions) The High c value is what stopped my ideas to solve 3Sum or similiar problems in amortized fast time. 

# MIS
We can construct this reduction-database pair for maximum independent set 
by focusing on the maximum degree of the graph.
While it is at least k, applying the inclusion-exclusion principle to the maximum degree vertex gives the
recursion relation: ```T(n) <= T(n-1) + T(n-k) + O(``` which has a smaller and smaller exponent as k grows.
I am under the impression the exponent is roughly ```lg(k)/k```.
So a runtime of at most 2^((lg(k)/k)*n) when all the database calls succeed.
When they don't succeed, it still has the same runtime bound 
as whatever the exponential backup algorithm is because the vertex count is shrinking in the reduction.
The database's size is at most  O( n^(kn) ) because there is kn/2 edges each of which has n^2 options. 
## Making the DB smaller
One might be able to make the database smaller by finding other heuristics which partially work in the sense that they have a good enough recursion until the graph has some new property.
  
# 0-1 LP
We may be able to construct a reduction-database pair for integer linear program where all the variables are either 0 or 1.
Algorithm:
```
\\ P is the linear program, center and R define an open ball containing all valid solutions
Find_Solution(P,center,R)  
  nearest_lattice_point = round( center )
  if distance_between(center,nearest_lattice_point) >= R :
    return no_solution
  if nearest_lattice_point satisfies P:
    return nearest_lattice_point
  new_center, new_R = shrink_Ball(center,R,nearest_lattice_point)
  return Find_solution(P, new_center, new_R)
```
The ball's radius R can start at any number greater than sqrt(n)/2.
If we track the center up to some precision epsilon (say 1/n or 1/n^2),
can one always find a smaller ball which contains the intersection of the previous ball
and the walls of a nearest lattice point? and furthermore, is there some delta such that the ball's radius always shrinks by at least delta? 
If both are yes, the algorithm runs in R/delta calls to a database of size (1/epsilon)^n .
If either are sometimes true, the algorithm might work if it gets lucky, but might reach a fail state.
