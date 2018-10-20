Just gibbs sampling
https://www.youtube.com/watch?v=mXgfRvRmDFI

Solution MCMC Gibbs sampling seed=1

See the PDF for arrows and shit that better shows what's going on.

Note that the observed values eliminate some of the probabilities in the table given. In this case, A is always true and E is always false and the columns and rows with the probabilities corresponding to those cases can be scratched.

![Removed rows](MCMC-graph.jpg)

The only probabilities that chage on each iteration is those who depend on the changed state. So if B changes, only B and D change If E changes, only E changes. If C changes, C, D and E change.

Besides D, the other columns only have two possible probabilities and so they can just be switched when the probability updates. \(Represented by the blue arrows\)

Observed Values -> These values are fixed, but not their probabilities if they have dependencies

Sample 0:

A | B | C | D | E
--|---|---|---|---
1 | 1 | 0 | 1 | 0

Based on Observed Values and Initial Values

A | B\|A | C\|A | D\|B,C | E\|C | P_old | P_new | ratio | rand | Accepted?
--|------|------|--------|------|-------|-------|-------|------|----------
1 | 1 | 0 | 1 | 0 
0.75 | 0.4 | 0.2 | 0.35 | 0.1 | .0021
1 | 0 | 0 | 1 | 0 
0.75 | 0.6 | 0.2 | 0.1 | 0.1 | .0021 | .0009 | .429 | .267 | .429 \> .267 \-> Yes
 | | | | | | | | | 
1 | 0 | 0 | 1 | 0 
0.75 | 0.6 | 0.2 | 0.1 | 0.1 | .0009
1 | 0 | 1 | 1 | 0 
0.75 | 0.6 | 0.8 | 0.05 | 0.2 | .0009 | .0036 | \> 1 | | Yes
 | | | | | | | | | 
1 | 0 | 1 | 0 | 0
0.75 | 0.6 | 0.8 | 0.95 | 0.2 | .0036 | .0684 | \> 1 | | Yes

Sample 1:

A | B | C | D | E
--|---|---|---|---
1 | 0 | 1 | 0 | 0

A | B\|A | C\|A | D\|B,C | E\|C | P_old | P_new | ratio | rand | Accepted?
--|------|------|--------|------|-------|-------|-------|------|----------
1 | 0 | 1 | 0 | 0
0.75 | 0.6 | 0.8 | 0.95 | 0.2 | .0684
1 | 1 | 1 | 0 | 0
0.75 | 0.4 | 0.8 | 0.65 | 0.2 | .0684 | .0312 | .4561 | .386 | Yes
 | | | | | | | | | 
1 | 1 | 1 | 0 | 0
0.75 | 0.4 | 0.8 | 0.65 | 0.2 | .0312 |
1 | 1 | 0 | 0 | 0
0.75 | 0.4 | 0.2 | 0.65 | 0.1 | .0312 | .0039 | .125 | .013 | .125 \> .013 \-> yes
 | | | | | | | | | 
1 | 1 | 0 | 0 | 0
0.75 | 0.4 | 0.2 | 0.65 | 0.1 | .0039
1 | 1 | 0 | 1  | 0
0.75 | 0.4 | 0.2 | 0.35 | 0.1 | .0039 | .0021 | .538 | .382 | .538 \> .382 \-> yes

Sample 2:

A | B | C | D | E
--|---|---|---|---
1 | 1 | 0 | 1 | 0
