HMM Viterbi
===========

WIP!

# Example

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


