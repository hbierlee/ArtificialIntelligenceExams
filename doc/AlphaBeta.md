# Alpha-Beta pruning
This is one of the easier questions (no calculations!) as long as you know what's going on and don't get confused. I don't have a better approach than how the script goes through the nodes.


## My pen-and-paper approach and general tips
- Leading principle: optimal play by both players
	- This means that, given the game tree, and consistent tie-breaking, there is only one possible outcome of the game (which we are going to find with this algorithm).
- Copy over the tree
- Denote for each tree level if it's the MIN or the MAX player's turn
- On the left of each node I write the current alpha value, on the right the current beta value
	- No value means `-Inf` for alpha, `Inf` for beta
- What are MIN/MAX players?
	- The MAX player tries to end up with the highest (most positive) value
	- the MIN player tries to end up with the lowest (most negative) value.
	- When going through the algorithm, it is important to keep in mind if we are on the layer of the MIN or the MAX player!
- What do these alpha/beta values mean?
	- Say we are at some node in the middle of the tree, and we explored some branches (but not yet all of them) down to the end-state
	- I think of the alpha value as the best known outcome MAX can hope for, given all the branches we explored.
	- If the alpha value is for example `-5`, that means we currently think the best choice for MAX will lead to an end-state of `-5`. This of course means MAX will lose, in fact, but taking another branch we already explored would lead to an even worse loss of `-23` or something.
	- However, since some branches from that node are still unexplored, this value might improve (to say `+3`, which means MAX will win).
	- So at each of their turns, the MAX player hopes to increase the alpha value of that node
	- Equivalent for Beta/Min, which hopes at each of their turns to decrease the beta value of each node
- Move down and up the tree in a depth-first search manner, copying over alpha/beta values along the way.
- Keep in mind when copying values:
	- When updating an alpha/beta value downward the tree to a child node, they become the child node's alpha/beta value, *respectively*
		- This makes sense, because when we move down the tree, we don't get any *new* end-state game information. So the values of the child node should be the same as the parent node.
	- When updating an alpha/beta value upward the tree to a parent node, they become the parent node's beta/alpha value, **respectively**. So going up 'mirrors' the values!
		- This makes sense: we go back one turn in the game, and pass back what the *other* player's best  because when we update the best choice the other player could make last turn
	- Values are not updated if they are not 'better' from the perspective of the player
		- So, alpha values only increase, and beta values only decrease
		- This makes sense, because if say the alpha value decreases at some node, it means we assume the player will make a move on that node that is worse than some previous move we found (that yielded the original alpha value). This would be non-optimal play.
- We prune the edge of any node when after updating the alpha/beta value, the alpha value becomes greater or equal to the beta value.
	- Note that this algorithm achieves exactly the same as regular min-maxing, but saves on time by not exploring (=pruning) branches that would never occur during optimal play by both players.
- Don't forget to mention when done what is the end-state of the game (assuming optimal play)

