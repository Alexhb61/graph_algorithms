# Basic Ideas
I want to analyze or accelerate predictions about some ODE system using discrete methods.
The two key ideas I'm using are the following:
1. Theoretical CS idea: Any finite precision process is also a finite state process. (you can list out all the states by enumerating all distinct points in the phase space)
2. If a problem can be fully described by ODEs and boundary conditions, it has the markov property ( with enough variables the process is memoryless).
#### Key Claim: Any finite precision discrete time simulation of an ODE system has a markov chain representation.
When the number of variables and (derivatives) is low and the amount of precision needed is low, the markov chain might be computationally feasible to analyze.
Note the state count is by default ``` 2^(V*P) ```(where V is the number of variables we need to track and P is the amount of precision each variable needs).
The first key research question is when the problem is slightly beyond computationally feasible:
#### How do you prune or condense the state space to keep the state count down?
The Second key research question is when a bunch of states are condensed into 1 state:
#### How do you compute the transition probabilites?
The third key research question is once the markov chain representation exists:
#### What matrix or graph theory phenomenon approximately captures the continuous phenomenon of interest.

# Examples:
## Space Trash Crash Prediction:
Each piece of Space Trash's trajectory in space has 6 variables (positition and velocity in 3 dimensions each).
Each trajectory does not require absurd precision. The moon landing was acheived with 16-24 bit computation.
The boundary conditions of the ODE are well defined: crash trajectories and escape trajectories can be computed.
The phenomenon of interest is collision trajectories, so that we can remove large pieces of space trash which are about to collide before they collide.
This can be roughly computed as the inner product of location distributions over time for two pieces of trash.
The boundary conditions will help reduce the dimensions, and zones of high interest could help condense less interesting parts of state space.
The transition probabilities could be calculated by uniformaly sampling over the range a point represents and calculating the possible new states.

## 3-Body Problem Periodic Solutions:
This problem by default has 18 degrees of freedom 3 objects * 3 dimensions * ( 1 quantity and its derivative).
### Formulation 1
However, we can remove many degrees of freedom by centering the center of mass,
and choosing the center of two masses with the third to form an axis with,
then defining all three masses to be in the xy plane.
This gives 3 positional degrees of freedom.
By choosing our reference frame to be center of mass and setting the net momentum to zero, we get 6 movement degrees of freedom.
By choosing the angular momentum to be a constant, we might be able to remove another degree of freedom.
So we have 8 dimensions of the ideal 3-body problem.
### Formulation 2 
We could focus a possibly more stable formulation by centering the center of mass,
and choosing the z axis to be the "direction" of the angular momentum of the system.
By choosing the 1st mass to exist in the xz plane, we get 5 degrees of freedom in position.
By applying the net linear momentum being zero constraint and the net angular momentum being constant constraint, we should have 3 remaining degrees of freedom in movement.
So we have 8 dimensions of the ideal 3-body problem
### Both formulation details
The boundary conditions include the phase space near the singularities which correspond to collisions.
The other big boundary condition is when the distances between the bodies trend towards infinity.
The Graph theory construct which overgeneralizes a periodic solution is a strongly connected component in the markov chain.
The Matrix construct which overgeneralizes a periodic solution is an eigenvector of the transition probability matrix.
We can the focus the search space on these portions of the problem.
Refining the Markov Chain in these spaces is easier done for the graph theory version.

## Restricted 3-Body problem Periodic solutions:
If we take formulation 2 of the 3-body problem, but place the restrictions of all 3 bodies being in the xy plane,
then we have 6 degrees of freedom in total.
This lower dimension problem is more feasible.
