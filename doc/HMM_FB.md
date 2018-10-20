HMM Forward-Backward
====================

WIP!!
- the solve F- and B-values solves are already usable though

# Example
```
> makeQuestionHMM_FB(seed=1)
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
[1] "Forward Values:"
[1] 0.5 0.5
[1] 0.210 0.585
[1] 0.1188 0.0498
[1] "Backward Values:"
[1] 0.1728 0.1644
[1] 0.19 0.22
[1] 1 1
[1] "State Probabilities:"
[1] 0.5124555 0.4875445
[1] 0.2366548 0.7633452
[1] 0.7046263 0.2953737
```


# Theory
## Markov Model
- The Markov property holds if each subsequent state S[t] of the system only depends on the last state S[t-1]
- This means that each subsequent state can be calculated with just a Transition matrix T, as follows:
	- `S[t] = S[t-1] * T`

## Hidden Markov Model and the Forward algorithm
- But what if we can't see the state, only the initial state `S[0]`?
- With the help of some observations at each time `O[t]`, and the Emission Matrix `E` (which tells us the probability for the state `S` given each possible observation `O`), we can still calculate `S` (or rather, the F-values `F` which is proportional to `S`, which is just as good because we can always `normalize(F)=S`)
- To calculate the F-values at each time step, from `0` through `t`:
	- `F[0]=S[0]`
	- `F[t]=F[t-1]*T*O[t]`
- You can see `F[t]` as the probability of each possible state, given observations `O[0]..O[t]` and S[0]

## Smoothing: Hidden Markov Model and the Forward-BACKWARD algorithm

- So, if we are a couple of steps in to the process, say at time `t=5`, and we calculated F-values `F[0..t]`. At this time, we now know more about some earlier state `t=2` than we did at the time we calculated `F[t=2]`. We calculated `F[t=2]` using `S[0], O[1]` and  `O[2]`, but now we have three additional observations `O[3], O[4]` and O[5]. We can use this to update our estimated state at `t=2` with this "future" knowledge.

# Solve
We are given `S`, `T`, `E` and `O[1,2]`. First we calculate the F-values `F[1,2]`, then B-values `B[1,2]` and then we combine them into our final estimated state probabilities.

## Forward probabilities
i for each F[1, i]


- ```F[1,1] = 
	(S[0,1] * T[1,1] + S[0,2] * T[2,1]) # regular markov chain transition S * T
	* E[1,O[1]]						# take into account the probability of seeing observation O[1] according to the emission matrix ```
	- In other words, take the dot-product of F[0] with T[1,], and multiply it with the probability of seeing O at time 1 (`=E[1,O[1]]`)
- `F[1,2] = (S[0,1] * T[1,2] + S[0,2] * T[2,2]) * E[2,O[1]]`
- `F[2,1] = (S[1,1] * T[1,2] + S[1,2] * T[2,2]) * E[1,O[1]]`
- `F[2,2] = (S[1,2] * T[2,2] + S[1,2] * T[2,2]) * E[2,O[1]]`
- etc..
- In general
	- Get `O = E[,0[t]]` (the observed column of `E`)
	- Get `F = S[t-1] * T`
	- Multiply each row in `F` with corresponding `O` value

More visually: in the blanks fill in the column of `F` and (observed) column of `E`.
```
          F[,t-1]             F[,t-1]                E[O[t]]
F[t] = [( ______  * T[1,1]  +  ______ *   T[2,1] ) * _______,  # values from the first column of T
        ( ______  * T[1,2]  +  ______ *   T[2,2] ) * _______]  # values from the second column of T
```


## Backward probabilities
0. Initialize `B[2]=[1,1]`
1. Start with empty `B[t]`. We are going to fill in the blanks by using `T`, `E`, last `O` and previously calculated `B`:
	```
	B[t] = [ ___ * ___ * ___ + ___ * ___ * ___ ] ,
	         ___ * ___ * ___ + ___ * ___ * ___ ] ]
	```
2. Copy over the `T` values, which line up exactly with the `T` matrix!
	```
	B[t] = [ T[1,1] * ___ * ___ + T[1,2] * ___ * ___ ] ,
	         T[2,1] * ___ * ___ + T[2,2] * ___ * ___ ] ]
	```
3. Get the last obtained B-values, `B[t+1]`. 'Quarter-flip' this vector and now they line up with the blanks for each row (and over the columns they are the same):
	```
	B[t] = [ T[1,1] * B[t+1,1] * ___ + T[1,2] * B[t+1,2] * ___ ] ,
	         T[2,1] * B[t+1,1] * ___ + T[2,2] * B[t+1,2] * ___ ] ]
	```
4. Get the correct column of `E` values, depending on `O[t+1]`. Again, quarter-flip this column so that in `B`, the columns are same, rows are different for these values:
	```
	B[t] = [ T[1,1] * B[t+1,1] * E[O[1,O[t+1]]] + T[1,2] * B[t+1,2] * E[O[2,O[t+1]]] ] ,
	         T[2,1] * B[t+1,1] * E[O[1,O[t+1]]] + T[2,2] * B[t+1,2] * E[O[2,O[t+1]]] ] ]
	```


## Combine to get smooth P-values
- Use element-wise multiplication on `F` and `B` (make sure `B` is the right way around, ending with a row of 1's!) to obtain `P*`
	- element-wise multiplication creates an matrix of the same dimensions as `F`/`B`, with each element being `P*[i,j] = F[i,j] * B[i,j]`
- Normalize `P*` over each row to obtain `P`
