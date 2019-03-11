* [Introduction](#introduction)
* [A* basic concept](#a*-basic-concept)
* [Admissibility](#admissibility)
* [Consistency](#consistency)
* [Link between the two](#link-between-the-two)
* [Implementation](#implementation)
* [Conclusion](#conclusion)



### Introduction

**Artificial intelligence** in its core strives to solve problems of enormous combinatorial complexity. Over the years, these problems were boiled down to search problems. A search problem is a computational problem where you have to find a path from point A to point B. In our case, we'll be mapping search problems to appropriate graphs, where the nodes represent all the possible **states** we can end up in and the **edges** representing all the possible paths that we have at our disposal.

So the next logical question would be:

> How do we convert a problem into a search problem?

#### Example

Let's say that you have to get through a maze. Any time we want to convert any kind of problem into a search problem, we have to define six things:

1. Set of all states we might end up in
2. Start and finish state
3. Finish check (a way to check if we're at the finish state)
4. Set of possible actions (in this case, direction of movement)
5. A traversal function (a function that will tell us where we'll end up if we go a certain direction)
6. A pricelist of all the possible actions

The maze problem can be solved by mapping the intersections to appropriate nodes (red dots), and the possible directions we can go to appropriate graph edges (blue lines). Naturally, we define the start and finish states as the intersections where we enter the maze (node A), and where we want to exit the maze (node B). 

![Maze with state nodes](F:\StackAbuse\Introduction to AI\Graphics\5-by-5-orthogonal-maze.png)



Now because we have a finished graph, we can discuss algorithms for finding a path from state A to state B. In simple cases, where the generated graph consists of a small number of nodes and edges, BFS, DFS and Dijkstra would suffice. However, in a real life scenario, because we are dealing with problems of enormous combinatorial complexity, we know we will have to deal with an **enormous** amount of nodes and edges (solving the Rubik's Cube for example). Therefore, we have to use an algorithm that is in a sense **guided**. That is where an **informed search algorithm** arises, **A\***.

**Informed search** signifies that the algorithm has extra information to begin with. For example, an uninformed search problem algorithm would be finding a path from home to work completely blind. On the flipside, an informed search problem algorithm would be finding a path from home to work with the aid of your sight (seeing what is closer to your destination) or a map (knowing exactly how far away every single point is from your destination).



#### A* basic concept

A* is based on using heuristic methods to achieve **optimality** and **completeness**, and is a variant of the best-first algorithm.

When a search algorithm has the property of **optimality**, it means it is guaranteed to find the best possible solution, in our case the shortest path to the finish state. When a search algorithm has the property of **completeness**, it means if a solution to given problem exists, the algorithm is guaranteed to find it.

Each time A* enters a state, it calculates *f(n)* (n being the neighboring node) for all the neighboring nodes, and then enters the node with the lowest value of *f(n)*. These values are calculated with the following formula: 

$$
\mathcal f(n) = \mathcal g(n) + \mathcal h(n)
$$

*g(n)* being the value of the shortest path from the start node to node *n*, and *h(n)* being a heuristic approximation of the node's value.

For us to be able to reconstruct any path, we need to mark every node with it direct relative with an optimal *f(n)* value. This also means that if we revisit certain nodes, we'll have to update their most optimal relatives as well, more on that later.

The efficiency of A* is highly dependent on the heuristic value *h(n)*, and depending on the type of problem, we need to use a different heuristic function. Construction of such functions is no easy task and is one the fundamental problems of AI. The two fundamental properties a heuristic function **can** have are ***admissibility*** and ***consistency***.

#### Admissibility and Consistency

A given heuristic function *h(n)* is admissible if it never overestimates the real distance between *n* and the finish node, so for every node *n* the following formula applies:
$$
h(n)\leq h^*(n)
$$
*h\*(n)* being the real distance between n and the finish node. However, if the function does overestimate the  real distance, but never by more than *d*, we can safely say that the solution that that function produces is of  accuracy *d* (doesn't overestimate the shortest path from start to finish by more than *d*). 



A given heuristic function *h(n)* is consistent if it's value for the finish node is 0 and for every 2 neighboring nodes *m* and *n*, the following formula applies:
$$
c(n,m)+h(m)\geq h(n)
$$
*c(n,m)* being the distance between nodes n and m. Additionally, if *h(n)* is consistent, then we know the optimal path to any node that has been already inspected. This means that this function is **optimal**.



**Theorem**: If a heuristic function is consistent, then it is also admissible.

**Proof by complete induction**

The induction parameter *N* will be the number of nodes between node *n* and the finish node *s* on the shortest path between the two.

**Base**: *kN=0

If there are no nodes between *n* and *s*, and because we know that *h(n)* is consistent, the following equation is valid:

$$
c(n,s)+h(s)\geq h(n)
$$

Knowing *h\*(n)=c(n,s)* and *h(s)=0* we can safely deduce that 

$$
h^*(n)\geq h(n)
$$
**Induction hypothesis**: N<k

We hypothesize that the given rule is true for every *N*<*k*



**Induction step: **

In the case of N = k nodes on the shortest path from *n* to *s* , we inspect the first successor (node *m*) of the finish node n. Because we know that there is a path from *m* to *n*, and we know this path contains k-1 nodes, the following equation is valid:
$$
ℎ^*(𝑛) = 𝑐(𝑛, 𝑚) + ℎ^*(𝑚) ≥ 𝑐(𝑛, 𝑚) + ℎ(𝑚) ≥ ℎ(𝑛)
$$
Q.E.D.



#### Implementation

```python

```







