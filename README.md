# Test item reusable workflow

This repository provides a reusable GitHub Workflow that lints, runs test items and deploys documentation for Julia packages. It only works with packages that use the test item framework.

## PRERELEASE

This is very experimental prerelease software. It might break, not work etc. I will update this notice once it is ready for everyone to use.

## Getting started

Add the following file as `.github/workflows/juliaci.yml` to the repository of your package:

```yml
name: Julia CI

on:
  push: {branches: [main,master]}
  pull_request: {types: [opened,synchronize,reopened]}
  issue_comment: {types: [created]}
  schedule: [{cron: 0 0 * * *}]
  workflow_dispatch:

jobs:
  julia-ci:
    uses: julia-vscode/testitem-workflow/.github/workflows/juliaci.yml@v1
    secrets:
      codecov_token: ${{ secrets.CODECOV_TOKEN }}
```

## Configuration

The `juliaci.yml` workflow accepts a number of configuration options that control on what Julia versions tests will be run. The following options are supported:
- `include-release-versions` (`true` or `false`, default `true`): run tests on the latest stable Julia version.
- `include-lts-versions` (`true` or `false`, default `true`): run tests on the latest long-term support Julia version.
- `include-all-compatible-minor-versions` (`true` or `false`, default `false`): run tests on all Julia minor versions that are compatible with the `[compat]` section in the package's `Project.toml`.
- `include-smallest-compatible-minor-versions` (`true` or `false`, default `true`): run tests on the smallest Julia minor versions that is compatible with the `[compat]` section in the package's `Project.toml`.
- `include-rc-versions` (`true` or `false`, default `false`): run tests on the latest release candidate Julia version.
- `include-beta-versions` (`true` or `false`, default `false`): run tests on the latest beta Julia version.
- `include-alpha-versions` (`true` or `false`, default `false`): run tests on the latest alpha Julia version.
- `include-nightly-versions` (`true` or `false`, default `false`): run tests on the latest nightly Julia version.
- `include-windows-x64` (`true` or `false`, default `true`): run tests on Windows x64.
- `include-windows-x86` (`true` or `false`, default `true`): run tests on Windows x86.
- `include-linux-x64` (`true` or `false`, default `true`): run tests on Linux x64.
- `include-linux-x86` (`true` or `false`, default `true`): run tests on Linux x86.
- `include-macos-x64` (`true` or `false`, default `true`): run tests on MacOS x64.
- `include-macos-aarch64` (`true` or `false`, default `true`): run tests on MacOS aarch64.
- `env` (JSON string): By passing a JSON string one can set environment variables for the Julia process that executes test items. For example `env: '{"FOO": "BAR"}'` would set an environment variable named `FOO` to the value `BAR`.
- `github_job_prep_script`: Path to a Julia file that is run once on each GitHub worker before tests are executed.

In the following example tests are run on release candidate versions if they are available:

```yml
name: Julia CI

on:
  push: {branches: [main,master]}
  pull_request: {types: [opened,synchronize,reopened]}
  issue_comment: {types: [created]}
  schedule: [{cron: 0 0 * * *}]
  workflow_dispatch:

jobs:
  julia-ci:
    uses: julia-vscode/testitem-workflow/.github/workflows/juliaci.yml@v1
    with:
      include-rc-versions: true
    secrets:
      codecov_token: ${{ secrets.CODECOV_TOKEN }}
```
