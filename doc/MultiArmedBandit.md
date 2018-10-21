Multi-Armed Bandit
==================

This one is insanely simple.

# Example

```
> makeQuestionMultiArmedBandit(seed=1)
We are testing click through rates on three different web layouts.At the current point, the dirichlet (beta) distributions associated with each layout have the parameters:
A. 8 4 
B. 9 6 
C. 8 6 
The first value is associated with not clicking through, the second clicking through.

A new person views the site. We generate samples from the distributions to determine which layout is used. The samples are:
A. 0.39,0.22,0.41,0.46,0.29 
B. 0.64,0.46,0.31,0.12,0.58 
C. 0.42,0.43,0.58,0.56,0.52 

When shown the website with the chosen layout, the person makes a purchase ('clicks through').

QUESTION: What are the new parameters of the three distributions after this event?
ANSWER:
The sample means are:
A. 0.354 
B. 0.422 
C. 0.502 
So we use layout 3.
Since the person clicked through, the updated parameters are:
A. 8 4 
B. 9 6 
C. 8 7 
```

# Solve
- Calculate the mean for each of the A, B and C sample sets: 0.354 for A, 0.422 for B and 0.502 for C
	- (add all the samples of layout X and divide by the number of samples to get the mean for X)
- Select the layout with the greatest mean (in this case that would be C)
- Update the parameters to get the answer:
	- If the user did not click through, increment the first parameter of the selected layout
	- If the user did click through, increment the second parameter of selected layout
	- All other parameters are unchanged
	- So in our case, the parameters of layout C go from (8,6) to (8,7)

