---
layout: default
---

## Network Flow Problem

Four nodes in the system network connected to each other.
~~~
Sets 
i /1*4/
CONEX(I,I) /I1.I2,I1.I3,I1.I4,I2.I4,I3.I4/
 ~~~
The objective function will be 

\\[ \sum_{i,j} xp_{i,j}-\sum_{i,j} xp_{j,i}=F_{i}  \\]

~~~
$title NETWORK FLOW PROBLEM

**  First, sets are declared:
**  Set I has four elements.
**  Subset CONEX is defined as a subset of set I.
**  The subset CONEX establishes the valid connections between nodes I.

SET
 I          set of nodes in the network  /I1*I4/
 CONEX(I,I) set of node connections  /I1.I2,I1.I3,I1.I4,I2.I4,I3.I4/;

**  The set of nodes I must be duplicated to refer to
**  its different elements in the same constraint.

 ALIAS(I,J)

**  Vectors of data are defined as parameters.
**  Data are assigned to vector elements.
**  FMAX(I,J) is a data matrix, declared as a parameter, where all
**  its elements have the same value (4). This is a compact way
**  to declare a matrix (instead of using a TABLE).

PARAMETERS
 F(I)   the input or output flow at node I
      /I1   7
       I2  -4
       I3  -1
       I4  -2/
 FMAX(I,J) maximum flow capacity of conduction going from I to J;

FMAX(I,J)=4;

**  Optimization variables are declared.

VARIABLES
  z      objective function variable
  x(I,J) the flow going from node I to node J;

**  Limits are stated on variables using previously defined data.

x.lo(I,J)=-FMAX(I,J);
x.up(I,J)=FMAX(I,J);

** Constraints are declared.

EQUATIONS
 COST        objective function
 BALANCE(I)  conservation of flow conditions;

**  The objective function is a function of flows between connected nodes.
**  This requirement is stated using the $CONEX(I,J) condition.
**  The four BALANCE equations only consider the flow between
**  connected nodes and again the $CONEX(I,J) condition is necessary.

COST ..         z   =E=  SUM(CONEX(I,J),x(I,J)) ;
BALANCE(I) ..   SUM(J$CONEX(I,J),x(I,J))-SUM(J$CONEX(J,I),x(J,I))  =E=  F(I) ;

**  The next two sentences define the netflow model, considering all the above
**  constraints, and direct GAMS to solve the problem using the lp solver.

MODEL netflow /ALL/;
SOLVE netflow USING lp MINIMIZING z;

~~~


Reference:Castillo, E., Conejo A.J., Pedregal, P., Garc\'{\i}a, R. and Alguacil, N. (2002). Building and Solving Mathematical Programming Models in Engineering and Science, Pure and Applied Mathematics Series, Wiley, New York.


[back](./)
