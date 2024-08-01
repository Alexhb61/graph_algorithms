# Goal 
Parallel in Time prediction of an Low variable analytic ODE system.
We want the algorithm to be polynomial work in terms of precision and duration of the prediction, but possibly exponential in terms of variable count.
We will be assuming the ODE system is analytic or nearly analytic; therefore,
we can assert that each euler iteration can be computed with registers on the order of the variable count.
# Strategy 1 
#### 1. Turn euler iterations of "analytic" ODE into division free Straight Line Program of low width
#### Keep numbers in form ```M * 2^e ``` where e is an integer, and M is a real number noninclusively between -1 and 1.
#### Downside possibly hard to parallelize base case (exponent terms)
## Multiplication
``` A * B = (M_A * 2^e_A) * (M_B * 2^e_B) = (M_A*M_B) * 2^(e_A + e_B) ```
We don't need to do much to keep the number in format for multiplication
## Addition
``` A + B = (M_A * 2^e_A) + (M_B * 2^e_B) = (M_A * 2^(-1+min(e_A-e_B,0)) + M_B * 2^(-1+min(e_B-e_A,0)) ) * 2^(max(e_A,e_B)+1) ```
We do need to do shenanigans to keep the format during addition
Note ``` max(e_A, e_B) + min(-e_A,-e_B) = 0  ``` is a used but not always obvious idenitity.
Also, addition distributes over max and min operation.
## Layers 
#### We let the input mantissas be (M_0;M_1;M_2;...;M_n) where each section is still a valid mantissa but has twice the precision of the previous section.
``` M = M_0 + 1/2*M_1 + ... +  1/2^(2^i-1)*M_i ```
and ```M_i``` has ```2^i``` bits of precision
#### MULTIPICAITON: ``` L_0 = 0; L_1 = M_0 * N_0 ; L_i =(M_0;...M_(i-2))*N_i + M_(i-1)*N_(i-1) + M_i * (N_0;...N_(i-2))```
#### Addition equal case: ```L_0 = 0 ;  L_1 = (M_0;M_1) + (N_0;N_1) ;  L_i = 1/2*(M_i + N_i)```
#### Addition off by 1 case ``` L_0 = 0 ; L_1 = (M_0;M_1) + N_0/2 ; L_2 = M_2/2 + (N_1;N_2); L_i = M_i/2 + N_i/4 ```
The last case requires to offset values from the computation of exponent
