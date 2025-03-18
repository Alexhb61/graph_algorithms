# Introduction
In computational complexity theory, one often attempts to solve a decision problem as an isolated algorithm.
However, when an algorithm is used as a subroutine in some other problem we might want to 
account out its startup costs or rare costs through amortized analysis. After thinking about the
sparsification lemma for k-SAT, I think there are ways to do that.

# Basic Idea:
We can solve a hard problem that is big in description size by reducing it to a problem which is small in size 
and then looking up the answer in a database (and computing the answer if the problem is new to the database)
Note, in order to get a non-uniform non-amortized speed-up, one would need the database to be sufficiently small

# k-SAT
We can construct this reduction-database pair for k-SAT by using the sparsification lemma.
which in 2^(epsilon*n) time reduce to instances of c-occurrences n-variables k-SAT.
The database's size is at most O(n^(cn) ) because each occurrence has n options.
Downside to using this technique is that the worst case work is 2^(s_k +epsilon)n
# MIS
We can construct this reduction-database pair for maximum independent set 
by focusing on the maximum degree of the graph.
While it is at least k, applying the inclusion-exclusion principle to the maximum degree vertex gives the
recursion relation: ```T(n) <= T(n-1) + T(n-k)``` which has a smaller and smaller exponent as k grows.
I am under the impression the exponent is roughly ```lg(k)/k```.
So a runtime of at most 2^((lg(k)/k)*n) when all the database calls succeed.
When they don't succeed, it still has the same runtime bound 
as whatever the exponential backup algorithm is because the vertex count is shrinking in the reduction
The database's size is at most  O( n^(kn) ) because there is kn/2 edges each of which has n^2 options. 
