### Introduction

**Artificial intelligence** in it's core strives to solve problems of enormous combinatorial complexity. Over the years, these problems were boiled down to search problems. A search problem is a computational problem where you have to find a path from point A to point B. In our case, we'll be mapping search problems to appropriate graphs, where the nodes represent all the possible **states** we can end up in and the **edges** representing all the possible paths that we have at our disposal.

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

A* is based on using heuristic methods to achieve **optimality** and **completeness**.

When a search algorithm has the property of **optimality**, it means it is guaranteed to find the best possible solution, in our case the shortest path to the finish state. When a search algorithm has the property of **completeness**, it means if a solution to given problem exists, the algorithm is guaranteed to find it.

Each time A* enters a state, it judges the values of all it's neighboring states (direct relatives of a given note in a graph), with the following formula:

$$
\mathcal f(n) = \mathcal g(n) + \mathcal h(n)
$$


