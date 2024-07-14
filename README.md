# Test item reusable workflow

This repository provides a reusable GitHub Workflow that lints, runs test items and deploys documentation for Julia packages. It only works with packages that use the test item framework.

## PRERELEASE

This is very experimental prerelease software. It might break, not work etc. I will update this notice once it is ready for everyone to use.

## Getting started

Add the following file as `.github/workflows/test.yml` to the repository of your package:

```yml
name: CI

on:
  push:
    branches:
      - main
      - master
  pull_request:
    types: [opened, synchronize, reopened]
  workflow_dispatch:

jobs:
  julia-ci:
    uses: julia-vscode/testitem-workflow/.github/workflows/testitemci.yml@main
    secrets:
      codecov_token: ${{ secrets.CODECOV_TOKEN }}
```

## Configuration

The `testitemci.yml` workflow accepts a number of configuration options that control on what Julia versions tests will be run. The following options are supported:
- `include-release-versions` (`true` or `false`, default `true`): run tests on the latest stable Julia version.
- `include-lts-versions` (`true` or `false`, default `true`): run tests on the latest long-term support Julia version.
- `include-all-compatible-minor-versions` (`true` or `false`, default `false`): run tests on all Julia minor versions that are compatible with the `[compat]` section in the package's `Project.toml`.
- `include-smallest-compatible-minor-versions` (`true` or `false`, default `true`): run tests on the smallest Julia minor versions that is compatible with the `[compat]` section in the package's `Project.toml`.
- `include-rc-versions` (`true` or `false`, default `false`): run tests on the latest release candidate Julia version.
- `include-beta-versions` (`true` or `false`, default `false`): run tests on the latest beta Julia version.
- `include-alpha-versions` (`true` or `false`, default `false`): run tests on the latest alpha Julia version.
- `include-nightly-versions` (`true` or `false`, default `false`): run tests on the latest nightly Julia version.

In the following example tests are run on release candidate versions if they are available:

```yml
name: CI

on:
  push:
    branches:
      - main
      - master
  pull_request:
    types: [opened, synchronize, reopened]
  workflow_dispatch:

jobs:
  julia-ci:
    uses: julia-vscode/testitem-workflow/.github/workflows/testitemci.yml@main
    with:
      include-rc-versions: true
    secrets:
      codecov_token: ${{ secrets.CODECOV_TOKEN }}
```
