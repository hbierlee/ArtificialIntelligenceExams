HMM Viterbi
===========

wip
- solve sections seems usable, looks correct
- could benefit from a rewrite tho

# Example
```
> makeQuestionHMM_Vit(seed=1)
[1] "Transition Matrix:"
               state(t+1)=False state(t+1)=True
state(t)=False              0.3             0.7
state(t)=True               0.4             0.6
[1] "Emission Matrix:"
            emission=False emission=True
state=False            0.6           0.4
state=True             0.9           0.1
[1] "Initial State:"
state(0)=False  state(0)=True 
           0.5            0.5 
[1] "Observations:"
emission(1) emission(2) 
      FALSE        TRUE 
[1] "Step 1: Path Node Probabilities"
[1] 0.120 0.315
[1] "Step 1: Path Step Origins"
[1] "TRUE"  "FALSE"
[1] "Path Node Probabilities"
[1] 0.0504 0.0189
[1] "Step 1: Path Step Origins"
[1] "TRUE" "TRUE"
[1] "Most probable path: FALSE TRUE FALSE"
[1] "Probabality: 0.0504"
```

# Theory
- [ok youtube video explaining it, didn't finish it myself (~15 mins)](https://www.youtube.com/watch?v=RwwfUICZLsA)
- we are going to find the most probable path given the `S0`, `T`, `E` and `O`
	- first from each possible state transition, find the most probable transition value
	- path-find your way back to the start to get the most probable path


# Solve
- For each time-step `t` we are going to find the next entry of the probability chain `P`
- `P[,0] = S0`

To find subsequent `P[,1], P[,2], ..` in the other probability chain:

0. Draw a (completely disconnected) graph of your state:
	```
	(S1)    (S1)    (S2)
	(S2)    (S2)    (S2)
	```
1. We are going to fill in the blanks by selecting correct `p`, `t` and `e` values
	```
	P[1,1] = max(
		 P     T     E
		___ * ___ * ___,
		___ * ___ * ___
	)
	```
2. For the `P` blanks: just copy over the previous `P` in the first column
	```
	P[1,1] = max(
		prevP[1] * ___ * ___,
		prevP[2] * ___ * ___
	)
	```
3. For the `T` blanks: just lay over the current `T` column (the current one being, if this is the first `p` we're calculating for `t`, it's the first column, etc..)
	```
	P[1,1] = max(
		prevP[1] * T[1,1] * ___,
		prevP[2] * T[2,1] * ___
	)
	```
4. Finally, for the `E` blanks, on both rows, it's going to be the same! Namely: `e=E[1,O[t]]`
	```
	P[1,1] = max(
		prevP[1] * T[1,1] * e(=E[1,O[t]]),
		prevP[2] * T[2,1] * e	 # notice E is the same on both rows!
	)
	```
5. Pick the largest of the two row values, => `P[1,1]`. Draw an arrow from S1 to either S1 or S2, depending on which argument was the largest! Write the value of `P[1,1]` next to the arrow.
6. Repeat for `P[1,2]`. Notice you change columns in the `T` matrix, and the `e` changes to `e=E[2,O[t]]`! 
7. Repeat for the next time slice `t=2`, of course using for `prevP` the last obtained `P`

Then select the end state with the highest `P`, and back-track using the graph, picking the highest probability edge each time. The visited nodes will be your highest probability path. The probability of your highest probability path is the probability of the terminal node.

