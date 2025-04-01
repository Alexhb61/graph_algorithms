# Intro
This document is interested in the exchange problem for lung transplants.
The Problem: Given potential directed donations of portions of lungs, how do you rearrange the objects being donated to obey medical ethics constraints and logistics feasibility constraints while minimizing graft-host disease and other risks?
This problem seems harder than the corresponding problem for kidneys, and this research is building on the successful operations research done with that problem.

## The Inspiration Source: The Problem of Kidney Transplant Chains
This mathematical formulation is inspired by and building on the work of the Kidney Transplant Chain Problem.
There are multiple papers on this subject.
A less technical explanation is available [here](https://www.kidneyregistry.org/for-donors/kidney-donation-blog/what-is-a-kidney-donation-chain/).

# The Problem of Lung Transplant Networks:
We will first layout the concrete ethics.
Then we discuss the logistically-difficult, medically-suboptimal, and computationally-difficult formulations of the problem ( each of which is optimal in the other two ways).
## The Discrete Parts:
A person's lungs can be safely discussed as 5 lobes (discussing it as more objects complicates healing and surgery).[Expert Testimony Needed]
However, when live lung transplants are considered they are most frequently done as 2 live donors donating one lobe each to a child. [CITATION NEEDED]
#### Let us take that as the safe donatable unit: 1 lobe. 
When deceased donors are included, the most common transplants are double lung transplant and single lung transplant. [CITATION NEEDED]
Thus the amount of lobe donations a person could receive as a variable between 2 and 5 through other means.
Because we are taking 1 lobe from health donors, I am under the impression that we should cap the amount of lobes received at 4 to be fair to donors. [Expert Input needed]
Similarly, the live donor version of a single lung transplant might be a single lobe transplant. [Expert Testimony Needed]
#### Let us take the above as the Ethically sound range of donations: 1 to 4 lobes received.
Lastly, a deceased donor has 5 lobes to donate not all of which need to go to the same patient.

## The Tit for Tat trade:
A directed potential donor is someone who would like to donate to a patient, but is not necessarily the best match for that patient. 
So, they will commit to giving a lung lobe if their directed beneficiary gets a lobe.
(Is this the best term for the donor type? or is directed incompatible donor a better name) [Citation Needed]
#### Let each sick patient find d  directed potential donors and need l lobes to be donated to them.
This would create a donor pool of directed potential donors.
There might be ethics reasons we need d to equal l, and their might be efficiency reasons that d should exceed l.
The following formulation actually allows donors to participate in multiple pools while still never being asked to donate twice.
## Solvable, Medically-Optimal, and Logistically-infeasible Formulation: weighted perfect b-matching
We can set this as a weighted b-perfect matching problem.
This problem can be solved optimally, and so we can compare how the feasibility or fancier constraints effect or don't effect the total medical benefit.
The weights in the graph represents the positive impact or quality of that potential donation.
This formulation will linearly add the value of all donations.
(It can not account for interaction benefits or downsides)
The b-value in the graph represents lobes given or or unlocked or kept.
#### For each sick patients p,
1. there is a vertex p of b-value l  ( allowing them to receive l lobes)
2. there is a vertex p' of b-value l ( allowing them to unlock l donors)
3. there is a weight 0 edge between p and p' (no benefit is acquired if they do not participate.)
4. p has positive weight incoming edges from donors d', a , or z each weighed to indicate the medical benefit.
(We might use multiple edges between a donor-patient to consider which lobe to donate and which location to put it in.)
5. p' has a loop on itself (this allows a patient to receive donations without their donors donating)
#### For each directed donor d,
1. there is a vertex d of b-value 1.
2. there is a vertex d' of b-value 1.
3. There is a weight 0 edge between d and d'
(allowing the donor to not participate)
4.  an weight 0 edge from their beneficiary p' to d
( this allows a directed donor to participate only if a person they want to benefit is receiving it)
#### For each altruistic donor a,
1. there is a vertex a of b-value 1.
2. There is a zero weight loop on a.
(allowing the donor to not participate)
#### For each deceased donor z,
1. There is a vertex z of b-value between 1-5 depending on health ie how many lobes can be seriously donated.
## Logistically-feasible, Computationally-feasible, medically-suboptimal Formulation: Maximal k-D matching
This formulation can definitely be run, and is definitely logistically feasible, but it might involve fewer transplants than other formulations.
Each patient is a vertex in the graph.
For each set of 3-5 patients (depending on how many lobes are being donated), we search through all small valid plans by treating it as a tiny matching problem (see above) on the patients and their directed donors, the best plan for a set of patients becomes a hyperedge in the graph (assuming at least one plan is valid). 
Even if we need to brute force search all plans for a valid one that is exponential in the size of donor pool (which we expect to be small).
Then we find a maximal matching in the hypergraph created by repeatedly picking the heaviest edge until the graph is empty.
We might instead look in the literature for algorithms which work on dense hypergraphs, or algorithms which approximate the maximum matching in polynomial time. 
This formulation is computationally feasible because it is polynomial with an exponent dependent on patient count.
This formulation is logistically feasible as all the plans are small and independent, and other constraints can be enforced by removing offending edges.
The tradeoff is that this formulation will only find a submaximum matching, and so might find a matching of size 1/3 to 1/5 the size of the best possible; and thus fewer patients would have transplants than other formulations.

## Logisticially-feasible, medically-optimal, but computationally-hard Formulation: Mixed integer linear program with maybe feasible constraints.
### Solving the harder problem:
This problem is likely NP-hard as a mixed integer linear program with many hidden constraints.
But might be feasible in practice because of the chaotic nature of immune systems.
A strategy similar to those with Kidney Transplants might work as both are mixed integer linear programs with many maybe-feasible constraints.
### Context for logistic constraints
A potential directed donor is only willing to go into surgery either after or simultaneously as their beneficiary.
The person whose pool they are in.
Thus, for some constraints, we need to view this as a directed graph or network where we merge verticies p and p' (likewise with d and d').
All edges from d' to p need to be directed from d' to p, and edges from d to p' need to be directed from d to p'.
Call this the Proposal network.
We can also think of it as a network of tasks where each transplant is a proposed task.
Directed edges are partial ordering constraints of tasks.
[Name consultation needed]
We call a constraint maybe-feasible if it is easy to detect the violation of the constraint, but hard to enumerate/list using standard computational complexity definitions of easy(polynomial time) and hard(exponential space).[Name consultation needed]
We call a constraint probably-feasible if it is easy to enumerate, but makes the problem no longer a matching problem.[Name consultation needed]

### Chest Cavity Constraints: 
For ethical waste regions, we might want to prevent having to do a ton of cutting of lobes.
So it might be worthwhile to add up the volume to be placed in either lung cavities and set some upper bound on the total volume placed in either cavity.
A lower bound can similarly be implemented if we include their original lungs(the non-participation edge) as meeting this lower bound.
These bounds are simple linear constraints and thus are probably-feasible.

### All or nothing Surgery:
For some patients it might be important that they get the new lung lobes all at once.
This can be implemented as the integrality constraint on the edge between p and p' so that it is either 0 or l in amount.
Or make it a boolean variable which is weighted l in the appropriate locations.
This constraint is probably-feasible.

### Corequisition/Simultaneity Feasibility constraints:
We will detect a set of surgeries that need to be simultaneous as a strongly connected component in the Proposal network.
The size of that strongly connected component in terms of edges correlates with the number of lobes that need to be simultaneously transfered.
Thus, we might need to bound this size because of limited resources.
This is an easy to detect structure (linear time) but is a subgraph and thus hard to list, And so is maybe-feasible.

### Plan length Feasibility constraints:
We will be able to estimate how long a plan is by measuring the length of sequential surgeries by looking at the strongly connected components condensation of the Proposal Network.
Because that condensation is a directed acyclic graph, we can detect the longest path in the dag with components weighted by duration or some similar metric.
In the formulation of the proposal network as a set of tasks, this is the critical path in the network.
We might need to bound the lengths of theses plans because of urgency constraints.
Again, this length can be computed for a plan in linear time, but is a subgraph feature; thus, this constraint fits into the category of maybe-feasible constraints.

### Plan size Feasability constraints:
We can detect a set of surgeries that need to approved or rejected (or cut down) as a whole as a weakly connected component in the Proposal Network.
I am referring to this as a plan. 
A plan can be cut along a point in a topological ordering of the strongly connected component condensation.
This size of that weakly connected component in terms of nodes is equal to the number of patients whose consent is needed on some level.
Thus, we might need to bound this size because of likelihood of backing out concerns.
This is an easy to detect structure (linear time) but is a subgraph, and so fits into our regime of maybe-feasible.

