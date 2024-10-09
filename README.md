# Devici Code Genius Action
[![Devici](https://img.shields.io/badge/Devici-red)](https://devici.com)
[![Code Genius](https://img.shields.io/badge/Code%20Genius-v0.9.1-blue?logo=go)](https://devici-code-genius-source.s3.us-east-2.amazonaws.com/v0.9.1/code-genius-v0.9.1.zip)

This GitHub Action runs [Devici Code Genius](https://devici.com/platform/code-genius) on your repository to generate a threat model.

## Getting Started

Use the following guide to set up Devici Code Genius with Repository Mode in GitHub Actions as a dedicated step in your pipeline: 

### How to add to your GitHub Actions pipeline

1. Get your API token to allow Code Genius to interact with Devici platform inside a pipeline runner. You can do this on:
**[Devici Admin](https://app.devici.com/admin/security) page** > **API Access** > **Manage API Access**

![api-access.png](docs/api-access.png)

2. Set up `CG_CLIENT_ID` and `CG_CLIENT_SECRET` secrets in GitHub repository/organization settings. [Learn how](https://docs.github.com/en/actions/security-for-github-actions/security-guides/using-secrets-in-github-actions).

3. _(Optional)_ Add `codegenius.yaml` file to your repository root for more fine-tuned detection (i.e. with some directories included/excluded). For example:

```yaml
   scenarios:
# Name of the thread model
- name: ThreatModelOne
  # Set true to include .gitignore content as ignore entities
  gitignore: false
  # Set relative paths to file or directories to be analyzed
  include:
    - # dir/path
  # Set relative paths to file or directories to exclude from analysis
  exclude:
    - # dir/path

```
- You can read more about codegenius.yaml [here](https://app.devici.com/code-genius#codegeniusyaml).


4. Create a file inside `.github/workflows/` directory in your repository root and put following example contents:

```yaml
# .github/code-genius.yaml

name: Run Devici Code Genius
on:
  # Choose between pull_request, push, fork etc. You can specify 
  # more than one scenario.
  pull_request:
    # Choose between branches, tags.
    branches:
      # Set the desired branches or leave empty to run for 
      # every branch.
      - 'development', 'staging'
    
jobs:
    run:
      name: Run Test PR
      runs-on: ubuntu-latest
    
      steps:
        # Step 1. Checkout the code
        - name: Checkout Code
          uses: actions/checkout@v4

        # Step 2. Run Code Genius Repo mode provided that API token 
        # was set in secrets and the scanning directory is '.'
        - name: Run Code Genius
          # Be sure to state the correct version (after '@'), 
          # or simply choose 'latest'
          uses: Devici-ThreatModel/code-genius-action@latest 
          env:
            # Make sure to include 
            CG_CLIENT_ID: ${{ secrets.CG_CLIENT_ID }}
            CG_CLIENT_SECRET: ${{ secrets.CG_CLIENT_SECRET }}
```

- You are free to configure the pipeline however you like! You can get more info about setting up a GitHub Actions pipeline [here](https://docs.github.com/en/actions/writing-workflows/workflow-syntax-for-github-actions).


- _Sidenote_. When Code Genius is set up to run on a PR, it will generate helpful messages such as one below, as well as create a status badge before merging.
![pr-message.png](docs/pr-message.png)
![pr-status.png](docs/pr-status.png)

If you have any questions please visit [app.devici.com/code-genius](https://app.devici.com/code-genius) for more info about Devici Code Genius. 

_Happy threat-hunting_!

## Defined Inputs

### `client-id` and `client-secret`

You can state Client ID and Client Secret for API access from the platform using action inputs instead of `env`. This input is not required if provided inside `env` statement of GitHub Actions.

Example usage:
```yaml
    - uses: Devici-ThreatModel/code-genius-action@latest
      with:
        client-id: ${{ secrets.CG_CLIENT_ID }}
        client-secret: ${{ secrets.CG_CLIENT_SECRET }}
```

### `print-tree`

You can set Code Genius Action to print the detected tree into the output log of GitHub Actions. Default is `true`.

Example usage:
```yaml
    - uses: Devici-ThreatModel/code-genius-action@latest
      with:
        print-tree: false # or 'true'
```

### `path`

You can overwrite the directory which will be counted as an entrypoint for Code Genius. Default is `.`.

Example usage:
```yaml
    - uses: Devici-ThreatModel/code-genius-action@latest
      with:
        path: "./your/path/inside/repo/here/"
```

### `github-token`

A GitHub Token Code Genius uses to integrate with GitHub ecosystem (look at PRs, create statuses and messages). This input is provided automatically via an action. Default is `${{ github.token }}`.
