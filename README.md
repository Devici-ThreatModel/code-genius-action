# Code Genius Action

This GitHub Action runs Code Genius on your code to generate a threat model.

## Inputs

### `verbose-level`

Verbosity level (0-2). This input is not required and the default value is "1".

### `client-id`

Client ID for API access from the platform. This input is required.

### `client-secret`

Client Secret for API access from the platform. This input is required.

### `path`

Path to the code to analyze. This input is required.

### `github-token`

GitHub Token. This input is required and the default value is '${{ github.token }}'.

## Usage

```yaml
- name: Run Code Genius
  uses: Devici-ThreatModel/code-genius-action@latest
  with:
    verbose-level: '1'
    client-id: 'your-client-id'
    client-secret: 'your-client-secret'
    path: 'path-to-your-code'
    github-token: '${{ github.token }}'
```
