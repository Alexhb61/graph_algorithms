# Introduction
This note will be an attempt to show NP = coNP.
Specifically, I will argue for the construction and correctness of a certificate
for a graph having a clique number bounded by some variable k.

#### Warning incomplete thoughts:
I thought this would be complete but in typing it up, I have seen that it isn't.

# Lemmas:
## Known Lemma:
The integer solutions to the linear program for the fractional clique number are the cliques of the graph.
## Needed Lemma:
This linear program can be rewritten as n + 2^n - 2 constraints of the form x_v >= 0 
or x_S <= clique_number(G[S]) where S is a proper nonempty subset of V(G).
### Claim: This LP contains all cliques.
Proof: When x is an indication vector of a clique, it clearly obeys the variable constraints.
Furthermore, Any Clique will still be a valid clique 
when intersected with a particular constraint for vertex set S,
and so it can not be larger then the maximum clique for G[S] .
### Claim: This LP cannot contain a fractional clique of weight 1 + clique_number(G)
Proof:
Let v be in ALL maximum cliques of G,
then ```1[V\u]x <= clique_number(G\u) ```is violated.
because the weight of the left hand side is at least 1 + clique_number(G) - 1
and the right hand side is clique_number(G) - 1.
Let x_v be fractional value e between 0 and 1,
then ``` 1[V\u]x <= clique_number(G\u) ``` is violated
because ``` 1 + clique_number(G) - e > clique_number(G) >= clique_number(G\u) ```

# Certificates:
## Case A: 
For some graphs and clique number bounds ```w(G) < k``` ,
we can show that they are true simply by finding a division of the graph into two nontrivial parts A and B.
such that A union B = V ; A intersect B = emptySet; and |A| , |B| > 0 .
And find one certificate showing ```w(G[A]) < b```, 
and another certificate showing ```w(G[B]) < k - b ```
for some nonegative integer b.
### Certificate Size:
The size of the certificiate can be bounded as ```T(n) <= n + T(n-c) + T(c) ```,
which by standard chip and bound techniques is ```O(n^2)```
## Case B:
When there is no Case A certificate, but the bound``` w(G) < k ```is still true,
we can construct a certificate of polynomial size 
by solving the LP in the needed lemma with an implicit orcale.
By implicit orcale, I mean if we had an orcale that chose the correct constraint every time,
we could use the ellipsoid method or similar orcale methods to find out that the constraint
```1Tx >= k ```and the needed LP contradict eachother.
The ellipsoid method will only call this implicit orcale polynomial many times.
Then we need to show that each of those constraints is valid.
We can do that because case A and case C does not apply 
which allows us to rewrite ``` w(G[A]) + w(G[B])  == k ```.
Then we can find constructive evidence that ``` w(G[B]) >= b ```.
Which lets us write that ``` x_A <= w(G[A]) <= k - b ```.

### Certificate Size:
The size of the certificate can be bounded as O(n*poly(n)) 
when poly(n) is the amount of calls to the orcale.

## Case C:
``` w(G[A]) + w(G[B]) > k ; w(G) < k  ```
Maybe there is a way to handle this case, but not in this draft.


