Scheduling

Basic steps:

0. (Optional) Draw a direction graph to represent the tasks and their dependencies
1. Calculate the earliest start (ES) of each task, starting with the start task and working your way forward in the graph. ES = max(duration(dependencies))
2. Finish has Latest Start (LS) = ES(finish)
3. Calculate the LS of each tast, this wime working your way backwards from the finish. LS(A) = min(ES(Tasks that depend on A))
4. Calculate the Slack of each node Slack = LS - ES

Then determine in which order to schedule the tasks, using minimum Slack as the priorty function.

Make a table to determine how to schedule the tasks according to constraints. 
1. Add each task in the order given by the priority function. 
2. Determine the start time first by placing it after any dependencies. 
3. Then make sure the required durable/reusable resources (saw/hammer) are available.
4. Make sure there is the required consumable resource (But I think the problem is defined such that this is never a constraint)
5. Add the task to the table and block the resources required during the duration of the task. If the task consumes a resource, put it in the slot before (representing consuming the resource at the start turn of the tast). If the the task produces a resource, add it in the last slot (the resource is produced at completion of the task)