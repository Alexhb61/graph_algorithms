## Introduction
The purpose of this note is to show that a very restricted graph problem 11LGGR is NC1 hard.
This is a slight improvement over the place I saw the problem introduced : https://ieeexplore.ieee.org/document/1663746
In that paper, they show that 11LGGR is TC0 Hard.
## Max 1 In-degree, Max 1 Out-degree Layered Grid Graph Reachability (11LGGR) Problem
The restrictions for this reachability problem are as follows:
1. Grid Graph - The verticies are on a grid (we can assign two natural numbers to each vertex) 
and verticies only connect adjacent verticies in the grid (a,b) to (a,b+1) or (a,b-1) or (a+1,b) or (a-1,b)
2. Layered - All edges are directed either south or east.
3. Max 1 In-degree - Each vertex only has one edge coming into it.
4. Max 1 Out-degree - Each vertex only has one edge leaving it.
## Montone Boolean Formula, The canonical complete problem for NC1
A formula which can be recursively defined in terms of logical-and's, logical-or's, and constant/input variables.
# The Reduction:
All formulas will be converted to a square grid.
Each formula has two possible graphs we might construct to represent it(the direct and second graph).
### What True/False looks like
If the formula evaluates to True, The direct corresponding grid graph will have a path from the Northwest Corner to the central South point.
If the formula evaluates to False, The direct corresponding grid graph will have a path from the Northwest Corner to the central East point.
If the formula evaluates to True, the second corresponding grid graph will have a path from the west central point to the South east corner.
If the formula evaluates to False, the second corresponding grid graph will have a path from the North Central point to the South East Corner
### The Constants True and false
Using a 3 by 3 grid for true and false with only 1 edge on the west/south border:
1. The direct graph for true will have 3 edges starting in the northwest corner: south, east, south.
2. The direct graph for false will have 3 edges starting in the northwest corner: south, east, east.
3. The second graph for true will have 3 edges starting in the center west point: east, south, east.
4. The second graph for false will have 3 edges starting in the center north point: south, south, east.
### Scaling up a formula (if its smaller than its sibling/term)
The direct graph scale up by 2x can be made by putting the original in the northwest corner 
then adding 2 paths (a south-east-south and a east-south-east) to the outputs to make it 2x.
The second graph scale up by 2x can be made by putting the original in the southeast corner 
then adding 2 paths (a south-east-south and a east-south-east) to the inputs to make it 2x.
### (A and B) Construction
Both graphs are 4 times bigger than the larger of A and B.
The direct graph of (A and B) can be constructed from one direct graph of A, one direct graph of B, and one second graph of A.
1. Place the direct graph of A in the northwest corner.
2. Place the direct graph of B immediately south of it, halfway to the east. (so that the true output of A is mapped to the input to B)
3. Place the second graph of A immediately to the east of B, lined up  (so that the false output of B feeds into the true input of second A)
4. Connect a path from the direct graph of A's false output into the second graph of A's false input. (east then south)
5. Connect a path from the true output of the direct graph of B to the true output of (A and B) (south-east-south)
6. Connect a path from the only output of the second graph of A to the false output of (A and B) (due east)
   
The Second graph of (A and B) can be constructed from one direct graph of A, one second graph of B, and one second graph of A.
1. Place the second graph of A in the southeast corner.
2. Place the second graph of B immediately to the west, halfway to the north. (so that the output of B is mapped to the true input of A)
3. Place the direct graph of A immediately to the north, lined up (so that the true output of A feeds into the false input of B)
4. Connect a path from the false output of the direct graph of A to the false input of the second graph of A.
5. Connect a path from the false input of (A and B) to the input of the direct graph of A.
6. Connect a path from the true input of (A and B) to the true input of the second graph of B.

### (A or B) Construction
Both graphs are 4 times bigger than the larger of A and B.
The direct graph of (A or B) can be constructed from one direct graph of A, one direct graph of B, and one second graph of A.
1. Place the direct graph of A in the northwest corner.
2. Place the direct graph of B immediately east of it, halfway to the south. (so that the false output of A is mapped to the input of B)
3. Place the second graph of A immediately south of B, lined up ( so that the true output of B is mapped to the false input of A)
4. Connect a path from the true output of the direct graph of A to the true input of the second graph of A.
5. Connect a path from the only output of the second graph of A to the true output of (A or B) (Due south)
6. Connect a path from the false output of the direct graph of B to the false output of (A or B) (east-south-east)

The second graph of (A or B) can be constructed from one direct graph of A, one second graph of B, and one second graph of A.
1. Place the second graph of A in the southeast corner
2. Place the second graph of B immediately to the north of A halfway to the west.( So that the output of B is mapped to the false input of A)
3. Place the direct graph of A immediately to the west of B, lined up ( so that the false output of A is mapped to the true input of B)
4. Connect a path from the true output of the direct graph of A to the true input of the second graph of A.
5. Connect a path from the only input of the direct graph of A to the true input of (A or B) (Due east)
6. Connect a path from the false input of the second graph of A to the false input of (A or B) (south-east-sout)

#### So you can turn the formula into a graph.
We can show that each construction is correct by a simple proof by cases on each construction on each evaluation of the subformulas.
By deeply inspecting the graph, we can see that it meets the maximum degree constraint.
Clearly, only east and south edges were used.

# Conclusion
I mostly wanted to write down this construction. I have the drawings of these graphs on paper.
At some later point, I hope to turn this into a tiny proper paper.
