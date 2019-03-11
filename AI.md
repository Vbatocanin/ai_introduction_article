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

This is a direct implementation of A* on a graph structure. The heuristic function is defined as 1 for all nodes for the sake of simplicity. The graph is represented with an adjacency list, where the keys represent graph nodes, and the values contain a list of edges with the the corresponding destination nodes.

```python
from collections import deque
class Graph:

    # example of adjacency list (or rather map) 
    # adjacency_list = {
    # 'A': [('B', 1), ('C', 3), ('D', 7)],
    # 'B': [('D', 5)],
    # 'C': [('D', 12)]
    # }

    def __init__(self, adjacency_list):
        self.adjacency_list = adjacency_list
        
    def get_neighbors(self, v):
        return self.adjacency_list[v]

    # heuristic function with equal values for all nodes
    def h(self, n):
        H = {
                'A' : 1,
                'B' : 1,
                'C' : 1,
                'D': 1
                }

        return H[n]

    def aStarAlgorithm(self, startNode, stopNode):

        # openList is a list of nodes which have been visited, but who's neighbors
        # haven't all been inspected, starts off with the start node
        # closedList is a list of nodes which have been visidet
        # and who's neighbors have been inspected
        openList = set([startNode])
        closedList = set([])

        # g contains current distances from startNode to all other nodes
        # the default value (if it's not found in the map) is +infinity
        g = {}
        
        g[startNode] = 0

        # parents contains an adjacency map of all nodes
        parents = {}
        parents[startNode] = startNode

        while len(openList) > 0:

            n = None

            # find a node with the lowest value of f() - evaluation function
            for v in openList:
                if n == None or g[v] + self.h(v) < g[n] +  self.h(n):
                    n = v;

            if n == None:
                print('Path does not exist!')
                return None

            # if the current node is the stopNode
            # then we begin reconstructin the path from it to the startNode
            if n == stopNode:
                reconstPath = []

                while parents[n] != n:
                    reconstPath.append(n)
                    n = parents[n]

                reconstPath.append(startNode)

                reconstPath.reverse()
                
                print('Path found:{}'.format(reconstPath))
                return reconstPath

            # for all neighbors of the current node do
            for (m, weight) in self.get_neighbors(n):

                # if the current node isn't in both openList and closedList
                # add it to openList and note n as it's parent
                if m not in openList and m not in closedList:
                    openList.add(m)
                    parents[m] = n
                    g[m] = g[n] + weight


                # otherwise, check if it's quicker to first visit n, then m
                # and if it is, update parent data and g data
                # and if the node was in the closedList, move it to openList
                else:
                    if g[m] > g[n] + weight:
                        g[m] = g[n] + weight
                        parents[m] = n

                        if m in closedList:
                            closedList.remove(m)
                            openList.add(m)

            # remove n from the openList, and add it to closedList
            # because all of his neighbors were inspected
            openList.remove(n)
            closedList.add(n)
 
        print('Path does not exist!')
        return None
```





#### Conclusion

A* is very powerful algorithm with almost unlimited potential. However, it is only as good as its heuristic function, which can be highly variable considering the nature of a problem.

