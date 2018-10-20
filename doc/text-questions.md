

Explain the two ways a planning graph can be used to provide a heuristic for A\*. \(2 marks\)

Answer:
1. The max-level heuristic: The number of levels between the initil state layer \(given by the state whose heuristic value is being estimated\) and the first state layer where all goal literals present.
2. The set-level heuristic: The number of levels between the initil state layer \(given by the state whose heuristic value is being estimated\) and the first state layer where all goal literals present AND there are no mutexs between any pair of goal literals.


Q:
What is the purpose of including randomness in the action-deciding process of a reinforcement learning system? \(1 mark\)

Answer:
Including randomless allows the system to occasionally chose to make actions that it does not consider optimal. This permits it to explore the effect of such actions on the environment and learn more about the system it is controlling.


Q:
Greedy Hill Climb suffers from the problem of local optima. Name and provide a brief explanation of three alternative local search strategies covered in this course that attempt to overcome or minimize this problem. \(3 marks\)

Answer:
Any three of these four answers is correct:
1. Greedy Hill Climb with random restarts. Here the greedy hill climb is run repeatedly from random initial positions.
2. Simulated Annealing. At each step a random neighbor of the current state is selected. If the neighbor has a higher fitness, the transition is made. If not, the transition will be made with a probability equal to the neighbors fitness divided by the current nodes fitness multiplied by a temperature parameter. The temperature parameter is initialized with the value 1 and is then reduced to 0 following a cooling schedule.
3. Local Beam Search. Multiple initial nodes are selected, each known as a particle. Where we have n particles, at each search step all neighbors and current nodes of these n particles are placed in a candidate set, and the particles move to the n nodes in this set with the highest fitness values.
4. Stochastic Local Beam Search. As local beam search, except that particles move to nodes in the candidate set probabilistically. Typically, a particle can move to an unoccupied candidate node with probability equal to the fitness of that node divided by the sum of the fitness values of all unoccupied nodes in the candidate set. Initially, all nodes in the candidate set are considered unoccupied \(including current nodes\). Once one particle moves to a node in the candidate set it is considered occupied.


Q:
What is PDDL? Explain all components of a PDDL problem. Be as precise and concise as possible.  \(4 marks\)

Answer:
PDDL stands for planning domain definition language. It is used to specify planning problems in a way that is easy to solve using search-based techniques. It works with database semantics, such that all objects are uniquely named.

The components of PDDL are:
  1. States. These are lists of positive literals \(where literals are logically atomic statements - i.e. without 'and', 'or', 'if... then', and 'not'\). Negation is by ommission.
  2. Actions. These contain lists of preconditions and effects. These  lists contain positive and negative literals and show what is required  to be true in a state for an action to be permitted, and what changes  are effected when an action is performed. Actions play the role of  transitions between states.
  3. Goal state specification. This is a list of positive and  negative literals specifying what must be true for a state to be  a goal state.
In addition, for a PDDL problem to be fully specified an initial state  must be given.


Q:
Assume you have a GAN where the discriminator network is a simple binary \(Genuine/Fake\) classifier. Briefly explain how the generator network is trained. \(2 marks\)

Answer:
During training, the generator network will generates fake data cases from random noise based on its parameters. These are passed to the discriminator for classification. The discriminator will give a probability value to the image being genuine/fake. To train the generator network, we take the probability the image is fake as the loss function \(we want to minimize the probability the discriminator assigns to a generated image being fake\), and proceed to take the derivatives of this with regard to the parameters of the generator network. These derivatives 'pass through' the discriminator network's parameters by the chain rule of derivation, but the discriminators weights are not adjusted.


Q:
Explain the steps involving the memory vector in a pass through a LSTM layer at time t. Mention what is done to the memory vector \(non-mathematically\) and/or what the memory vector is used for in each of these steps. Make reference to the input at time t, and the outputs of time t-1 and t. \(2 marks\)

Answer:
The joint vector of the input at time t and the output at time t-1 is used \(in conjunction with the weights of the LSTM unit\) to update the memory vector. This has three steps: \(i\) The joint vector determines which elements in the memory vector should be 'forgotten', and to what degree; \(ii\) The joint vector determines which elements of the memory vector should be updated, and to what degree; and \(iii\) The joint vector determines the values that should be used to update the memory vector. This updated memory vector is then used in conjunction with the joint vector to determine the output of the LSTM at time t.


Q:
Under what conditions could a depth-first search FAIL to find a solution \(in a finite search space with at most a single edge between any two nodes\)? \(1 mark\)

Answer:
When the search space contains loops.


Q:
Give a basic explanation \(as per what was discussed in the course\) of the bias and variance components of expected error and their relationship to model complexity. \(3 marks\)

Answer:
The bias component of expected error is the expected error arising from the model being too simple to model the system well. The variance component of expected error is \(in a simplified sense\) the expected error arising from the model overfitting to data. As model complexity increases, the bias component of expected error decreases and the variance component increases.


Q:
Explain iterated deepening. \(2 marks\)

Answer:
Iterated deepening performs a depth first search to a designated depth controlled by a parameter t. States at this depth treated as if they had no outgoing transitions. The terminal depth parameter t is initially set to 1. If a depth constrained search terminates without finding the goal state, the terminal depth parameter is increased \(typically by 1\) and a new search is begun.