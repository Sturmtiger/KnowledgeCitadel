Workflow runs when triggered by an external event from another service.

Use cases:
- Triggering a build when a new issue is created on GitHub
- Deploying code to a server when a new version is released. ==Need more info==

## Example

```
on:
  webhook:
    url:https://example.com/my-webhook
```