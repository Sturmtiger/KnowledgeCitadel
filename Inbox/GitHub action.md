#github #action

An action is a script that you invoke within a [[GitHub workflow|workflow]].

```
on:
  push:
    branches: [ main ]

jobs:
  deploy:
    name: Deploy  # Optional name
    runs-on: ubuntu-latest

  steps:
    - uses: actions/checkout@v3  # Action may be a separate repo or be placed in your repo with the workflow using it.
    - name: Install dependencies
      run: npm install
    ...
```