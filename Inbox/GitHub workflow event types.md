#github #event 

There's a lot of them and, but for your certification you need to know the most common ones.
- Push events (pushing code into a branch, merging a PR. The push and a commit are events that can trigger workflows). Use cases: Running tests after every push occurs to a branch, Deployment code to a staging environment
- Pull request events
- Issue events
- Release events
- Workflow dispatch events (can be triggered ad hoc or manually)
- Scheduled events (cron scheduling)
- Webhook events (an HTTP request from an external service to trigger off GitHub, e.g. you can have a build happening in AWS/Azure or another build system and the results from there would then send an HTTP webhook into your repo, and that would be an event that then trips off (triggers) a particular workflow file)
## workflow.yaml file example
```
# Workflow runs when a new commit is pushed to the main or development branch.

on:
  push:
    branches:
      - main
      - development 
```

`on` is CI trigger.
Anything below `on` are the conditions that determine if that workflow runs.