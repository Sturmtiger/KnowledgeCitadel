#github #github-workflow 

Jobs are units of work within a workflow, each with its own [[GitHub workflow job step|steps]].

A [[GitHub workflow|workflow]] may have multiple jobs.

If the jobs do not have any dependencies, then they're going to be run **in parallel** on separate [[GitHub runner|runners]]. That's why you specify your runners for each job.