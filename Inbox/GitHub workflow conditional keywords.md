#github #github-workflow 

Conditional keywords are used to control the execution of [[GitHub workflow job|job]] steps.

### Job keywords
Other than `if` and `else` keywords there's another important one - `needs`.
`needs` is used to specify that a [[GitHub workflow job|job]] depends on another [[GitHub workflow job|job]] completing.

```yml
jobs:
  initial_job:
    ...
  conditional_job:
    needs: initial_job
    ...
```

In this example, `initial_job` and `conditional_job` won't run in parallel, because `conditional_job` requires `initial_job` to complete first.