# Workflow keep alive by commit
GitHub Action to prevent GitHub from [disabling scheduled workflows due to 
repository inactivity][1].

[1]: https://docs.github.com/en/actions/learn-github-actions/usage-limits-billing-and-administration#disabling-and-enabling-workflows

This Action runs only every "n" Days. It updates itself to adjust the next run date.

## Usage:

Create a workflow similar to:

```yaml
name: keep alive by commit
on:
  workflow_dispatch:
  push:
  schedule:
    # This (cron) line must be exactly as given
    #        | | only this values (Minute and Hour) may be different
    - cron: '0 0 * * *' # Modified by workflow on on every run
jobs:
  workflow-keepalive:
    runs-on: ubuntu-latest
    steps:
    - name: Clone the repository
      uses: actions/checkout@v4
      with:
        ref: ${{ github.head_ref }}
        token: '${{ secrets.PAT }}' # must be a token with write access and expiration (otherwise pushing from workflow isn't possible)
    - uses: gneiss15/WorkflowKeepAliveByCommit@v1
    # Optional
    #  with:
    #    Days: 45
```

