#github #github-workflow 

It may be used for:
- Performing hourly backups
- Running nightly builds
## Example
```
on:
  schedule:
    - cron: '0 0 * * *'  # Every day at midnight
```