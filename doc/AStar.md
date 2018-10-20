AStar
=====

# Example
```
"Edges:"
      Start A B C D E Goal
Start     0 2 0 0 6 0    0
A         0 0 4 0 2 0    0
B         0 0 0 4 0 0    0
C         0 0 0 0 4 0    0
D         0 0 0 0 0 4    0
E         0 0 0 0 0 0    3
Goal      0 0 0 0 0 0    0
"Heuristic:"
6 6 8 6 4 2 0
"Turn: 1"
"Next: 1 ( 0 - 6 - 1 )"
"Frontier:"
"   2 ( 2 - 6 - 1,2 )"
"   5 ( 6 - 4 - 1,5 )"
"Turn: 2"
"Next: 2 ( 2 - 6 - 1,2 )"
"Frontier:"
"   3 ( 6 - 8 - 1,2,3 )"
"   5 ( 4 - 4 - 1,2,5 )"
"Turn: 3"
"Next: 5 ( 4 - 4 - 1,2,5 )"
"Frontier:"
"   3 ( 6 - 8 - 1,2,3 )"
"   6 ( 8 - 2 - 1,2,5,6 )"
"Turn: 4"
"Next: 6 ( 8 - 2 - 1,2,5,6 )"
"Frontier:"
"   3 ( 6 - 8 - 1,2,3 )"
"   7 ( 11 - 0 - 1,2,5,6,7 )"
"Total Turn: 4"
"Cost: 11"
"Route: 1,2,5,6,7"
```

# Solve
1. Draw graph
	- IMPORTANT: the graph is directed!
	- [Unnecessary, but the answers use numbers instead of node names (Start = 1, A = 2, B = 3..) as node_id]
2. Calculate Heuristic for all nodes
	- get lowest cost edge `minH [=> minH = 2]` 
	- start with current heuristic, which is seven zeroes: `h = [0, 0, 0, 0, 0, 0, 0]`
	- `h[7] = 0`
	- Now for each node i of the rest of the nodes in reverse order (from 6 to 1):
		- copy all the values of h over to the next row, except current node:
		```
			0 0 0 0 0 0 0
			0 0 0 0 0 _ 0
		```
	- Mark all the neighbors of node i in your current h:
		```
			0 0 0 0 0 0 0
			            ^
			0 0 0 0 0 _ 0
		```
	- Your current node will be the `minH` plus the minimum value of all the marked nodes.
		```
			0 0 0 0 0 0 0
			            ^
			0 0 0 0 0 2 0
		```
	- Faster alternative: Determine `hMin`. Start at the Goal node, write down 0 (so `h(Goal)=0`. Follow all the arrows in the *opposite direction* to find the nodes from which Goal can be accessed (`inverse_neigbors`). Calculate `h(inverse_neigbors) = h(Goal) + hMin`. Repeat until you figured out `h` for all the nodes in the graph.
3. Now we're going to start the 1st iteration of the algorithm. Construct the Next and the Frontier sets, which will contain node objects of the format: `node_id (path_cost(node) - heuristic(node) - path(node))`.
	- `Next = Start = 1 (0 - 6 - 1)`
	- `Frontier = {unvisited_neighbors(Start)} [=> Frontier = {2 (2 - 6 - 1,2), 5 (6 - 4 - 1,4)}]`
	- Note: Frontier is a set (because its order does not matter!)
4. Calculate the costs for each node in Frontier, by adding the heuristic and the path_cost together:
	cost(node) = heuristic(node) + path_cost(node) [=> cost(2) = 6 + 2 = 8]
5. Update Next and Frontier:
	- `Next = min(cost(each node in Frontier)) # get minimal cost node from Frontier`
	- `Frontier = Frontier - Next # remove the minimal cost node from Frontier`
	- `Frontier = Frontier + unvisited_neighbors(Next) # add neighbors of new Next to the Frontier set`
	- IMPORTANT: when adding neighbors to the Frontier that are already in the Frontier, replace it ONLY IF the cost is lower. Otherwise ignore it.
	- IMPORTANT: Also ignore nodes already visited nodes (as the `unvisited_neighbors` function implies)
	- IMPORTANT: the path_cost field of a node is always the sum of the path_cost of the previous Next node! It's a cumulative sum. You can always check this by adding all the costs of the edges in `path(node)`
6. Go back to step 3. until `Next = Goal`, or until Frontier is empty






