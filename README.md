# Myks setup action

A GitHub Action to setup [myks](https://github.com/mykso/myks) in your workflow.

## Usage

```yaml
- name: Setup myks
  uses: mykso/setup-action@v3.0.6
  with:
    # myks version to install, `latest` if not specified
    # check for available versions at github.com/mykso/myks/releases
    myks: v4.11.0
    # GitHub token to avoid rate limits (optional)
    token: ${{ secrets.GITHUB_TOKEN }}
```

> [!IMPORTANT]
> Always pin tools to a specific version to avoid unexpected issues.
> Use Renovate or Dependabot to keep your versions up to date automatically.
> See the example below for a Renovate configuration.

## Inputs

| Input   | Description                       | Required | Default  |
| ------- | --------------------------------- | -------- | -------- |
| `myks`  | [myks version][1] to install      | No       | `latest` |
| `token` | GitHub token to avoid rate limits | No       | `''`     |

## Example

```yaml
name: Use myks
on:
  pull_request:

jobs:
  test:
    runs-on: ubuntu-latest
    env:
      # renovate: datasource=github-releases depName=mykso/myks
      MYKS_VERSION: v4.11.0
    steps:
      - uses: actions/checkout@v5.0.0

      - name: Setup myks
        uses: mykso/setup-action@v3.0.6
        with:
          myks: ${{ env.MYKS_VERSION }}
          token: ${{ secrets.GITHUB_TOKEN }}

      - name: Run myks
        run: myks --help
```

### Keeping Versions Up to Date with Renovate

Here is a minimal Renovate configuration to keep both the action and myks versions pinned and up to date:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["config:best-practices", "customManagers:githubActionsVersions"]
}
```

[1]: https://github.com/mykso/myks/releases
