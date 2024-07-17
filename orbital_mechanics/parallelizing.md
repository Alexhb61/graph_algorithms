# The Easy Problem
About a year ago, a friend of mine posed the simple request of could I help his friend parallelize orbital mechanics.
The Paper whose parallelization I was trying to beat was this: GPGPU IMPLEMENTATION OF PINES’ SPHERICAL HARMONIC GRAVITY MODEL.
The concrete problem was to parallelize one section of code which was a 2nd order linear recurrence relation with dynamic constants.
That subproblem can be formulated as a large sequential product of 2 by 2 matricies, and solved in log depth.
I'd have to dig into the Pines method again to show the speedup more concretely.
The PINES' SPHERICAL HARMONIC GRAVITY MODEL can be parallelized in an iteration sense. Neat.
This model generates the precise acceleration vectors which then are used to iterate foward the position and velocity vectors.
This made me consider the harder problem: Can one fully parallelize an orbital prediction method?

# The Harder Problem:
How can one parallelize an orbital mechanics model's iterative method?
Say for the purpose of tracking space trash and predicting collisions in advance?

## Pre-existing strategies:
I'm aware of but less interested in strategies which decrease the accuracy of a model in order to speed up computation because for the most part those are trading less work for less accuracy.
I'm interested in trading more work for less time.

## Exploiting Low Space/Low Memory: Markov Chain Representation of Phase Space
One angle to attack the problem is to notice that this ODE system has only 6 variables. Orbital Computation can be done with low bit precision say 14bits.
So we could treat the iterator as a low space(memory) computation and apply a technique from Savitch's Theorem: Constructing the graph of machine configurations.
This doesn't quite become feasible as 2^84 states is more than an exabit of states.
However, If one was to slowly increase the bit complexity while pruning(or not respecifying) states which are on escape or de-orbit trajectory, the "dimension" of the problem might be between 3 and 5 dimensions.
I think at least 3 dimensions because you can choose a place above the earth and a direction.
I think at most 5 because there is an escape/crash velocity at all locations.
Essentially, this strategy creates a markov chain representation(or approximation) of the phase space of a ODE problem.
Once you have a markov chain representation, you can raise that (likely sparse) state transition matrix to higher and higher powers. 
Then you can use those higher powers to make long duration predictions of a piece or collection of space junk's orbit via matrix vector multiplication.
This would be a neat idea to explore further.

## Exploiting Low Space/Low Memory: Low Width Programs
Because we can parallelize an iteration of the problem, we can rewrite the algorithm as a low width program. (A piece of code with few registers just a lot of constant)
[citation or demo needed]
When multiplication is limited in some ways, there are pre-existing methods to parallelize these programs. [citation needed]
When we have register-register multiplication such as in this program, those techniques fail.
If we were working with integers, a parallelization technique which would work is the following:
### Integer Technique:
Solve the program (and all of its intermediary states) mod 2 (or prime p)
by constructing the graph of machine configurations and using quick parallel graph theory techniques.
Then we can have registers with different positional values where the original addition gets replaced with addition done position wise but multiplication involves moving the positions of the output as appropriate.
(position i gets multiplied by position j and fed into the sum for postition i + j)
(That description might be too vague to understand)
This circuit would have arithmetic degree at most P if we use at most P position numbers.
SO, there are parallelization techniques that would work for these low degree circuits. [citation needed]
### Using P-adic Integers
In order to use this technique on real numbers, 
It seems possible to map fixed precision reals to the p-adic integers, (1/2 = a specific expansion)
and then run the computation in multiple p-adic systems. 
But it isn't clear to me, how one might either find a pattern in the p-adic expansion and extract a rational.
Or how one might locate a real number via multiple p-adic approximations.
