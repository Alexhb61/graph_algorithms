# The Problem of Lung Transplant Networks:
## The Discrete Parts:
A person's lungs can be safely discussed as 5 lobes.
However, when live lung transplants are considered they are most frequently done as 2 live donors donating one lobe each to a child.
Let us take that as the safe donatable unit: 1 lobe. And the amount of donations a person needs to receive as a variable between 2 and 4.

## The Tit for Tat trade:
Let each sick patient find d  tit for tat potential donors and need l lobes to be donated to them.
This could allow more flexibility. 

## Partial formulation
We can set this up so far as a weighted b-perfect matching problem Linear Program.
I refe
#### For each sick patients p,
1. there is a vertex p of b-value l  ( allowing them to receive l lobes)
2. there is a vertex p' of b-value l ( allowing them to unlock l donors)
3. there is a value 0 edge between p and p'
4. p has positive weight incoming edges from donors d' or a
5. For each of the donors in their donor pool:  an weight 0 edge from p' to d
6. p' has a loop on itself (this allows a patient to receive donations without their donors donating)
#### For each potential donor d,
1. there is a vertex d of b-value 1.
2. there is a vertex d' of b-value 1.
3. There is a weight 0 edge between d and d' (allowing the donor to not participate)
#### For each altruistic donor a,
1. there is a vertex a of b-value 1.
2. There is a zero weight loop on a. (allowing the donor to not participate)

## All or nothing Surgery:
For some patients it might be important that they get the new lung lobes all at once.
This can be implemented as the integrality constraint on the edge between p and p' so that it is either 0 or l in amount.

## Feasibility constraints:
A potential tit for tat donor is only willing to go into surgery either after or simultaneously as their beneficiary. The person whose pool they are in.
Thus we need to view this as a directed graph or network where we merge verticies p and p' (likewise with d and d').
All edges from d' to p need to be directed like that, and edges from d to p' need to be directed like that.
We will then detect a set of surgeries that need to be simultaneous as a strongly connected component in the constructed network.
This is an easy to detect structure (linear time). And so is a set of hard to enumerate but easy to detect violated constraints.

# Solving this problem:
This problem is likely NP-hard as a mixed integer linear program with many hidden constraints.
But might be feasible in practice because of the chaotic nature of immune systems.
A strategy similar to those with Kidney Transplants might work as both are mixed integer linear programs with many hidden constraints.
