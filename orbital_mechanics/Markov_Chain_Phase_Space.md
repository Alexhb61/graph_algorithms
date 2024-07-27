# Basic Ideas
I want to analyze or accelerate predictions about some ODE system using discrete methods.
The two key ideas I'm using are the following:
1. Theoretical CS idea: Any finite precision process is also a finite state process. (you can list out all the states by enumerating all distinct points in the phase space)
2. If a problem can be fully described by ODEs and boundary conditions, it has the markov property ( with enough variables the process is memoryless).
#### Key Claim: Any finite precision ODE system has a markov chain representation.
When the number of variables and (derivatives) is low and the amount of precision needed is low, the markov chain might be computationally feasible to analyze.
Note the state count is by default ``` 2^(V*P) ```(where V is the number of variables we need to track and P is the amount of precision each variable needs).
The first key research question is when the problem is slightly beyond computationally feasible:
#### How do you prune or condense the state space to keep the state count down?
The Second key research question is when a bunch of states are condensed into 1 state:
#### How do you compute the transition probabilites?
The third key research question is once the markov chain representation exists:
#### What matrix or graph theory phenomenon approximately captures the continuous phenomenon of interest.

## Examples:
### Space Trash Crash Prediction:
Each piece of Space Trash's trajectory in space has 6 variables (positition and velocity in 3 dimensions each).
Each trajectory does not require absurd precision. The moon landing was acheived with 16-24 bit computation.
The boundary conditions of the ODE are well defined: crash trajectories and escape trajectories can be computed.
The phenomenon of interest is collision trajectories, so that we can remove large pieces of space trash which are about to collide before they collide.
This can be roughly computed as the inner product of location distributions over time for two pieces of trash.
The boundary conditions will help reduce the dimensions, and zones of high interest could help condense less interesting parts of state space.
