# Semver PR Label Check Workflow

This reusable GitHub Actions workflow enforces the presence of a valid semver label
on every PR to your repository.

* [Introduction](#introduction)
* [Setting up the semver PR label check workflow](#setting-up-the-semver-pr-label-check-workflow)
  * [1. Create the required change type labels in GitHub](#1-create-the-required-change-type-labels-in-github)
  * [2. Add the semver PR label check workflow file to your repository](#2-add-the-semver-pr-label-check-workflow-file-to-your-repository)
* [Troubleshooting](#troubleshooting)
  * [Workflow does not trigger](#workflow-does-not-trigger)
  * [Missing labels](#missing-labels)
  * [Permissions issues](#permissions-issues)
* [Handling errors](#handling-errors)
* [Change type labels](#change-type-labels)
  * [`major-change`](#major-change)
  * [`minor-change`](#minor-change)
  * [`patch-change`](#patch-change)
  * [`internal-change`](#internal-change)
  * [`release`](#release)

## Introduction

This workflow ensures every PR has exactly one required semver label, blocking the PR
from merging until a valid label is applied.

The label categorizes the change type, ensuring the artifact version increments
correctly according to [Semantic Versioning rules](https://semver.org).

One of the following semver labels is required:

* `major-change`
* `minor-change`
* `patch-change`
* `internal-change`
* `release`

See [Change Type Labels](#change-type-labels) for detailed definitions of each change
type.

## Setting up the semver PR label check workflow

### 1. Create the required change type labels in GitHub

Create the semver labels in your repository by navigating to:

```url
https://github.com/<owner>/<repo>/labels
```

and then add the labels listed in the [Change type labels](#change-type-labels) section.

### 2. Add the semver PR label check workflow file to your repository

Create a workflow file in your project that references the reusable workflow provided
by this repository. We recommend naming the file
.github/workflows/semver_pr_label_check.yml. Replace `main` in `branches: [main]` with
your repository’s default branch name (e.g., `master`).

The workflow file should contain the following:

```yaml
name: Semver PR Label Check

on:
  pull_request:
    branches: [main]
    types: [opened, synchronize, reopened, labeled, unlabeled]

jobs:
  run_semver_pr_label_check:
    uses: main-branch/semver_pr_label_check/.github/workflows/semver_pr_label_check.yml@main
    secrets:
      repo_token: ${{ secrets.GITHUB_TOKEN }}
```

The `repo_token` (typically `${{ secrets.GITHUB_TOKEN }}`) is an automatically
generated secret provided by GitHub Actions. It is used to authenticate the workflow
with GitHub and must have permissions to read and modify pull request labels.

By default, the `pull_request` event only triggers a workflow for the `opened`,
`synchronize`, and `reopened` activity types. The `labeled` and `unlabeled` activity
types are added so this workflow is also run when a label is added or removed from a
PR.

## Troubleshooting

### Workflow does not trigger

Ensure that the workflow file is correctly named and located in the
`.github/workflows` directory of your repository. Verify that the `pull_request`
event is configured correctly to listen for relevant actions.

### Missing labels

Make sure the required semver labels (`major-change`, `minor-change`, `patch-change`,
`internal-change`, `release`) are added to your GitHub repository.

### Permissions issues

Ensure that the GitHub token used (`repo_token`) has the necessary permissions to add
or modify labels on pull requests.

## Handling errors

If the workflow fails because the semver label is missing or incorrect, it will block
the PR from merging.

You may choose to add additional steps in your calling workflow to notify the team or
provide feedback to the PR author, such as sending a Slack message or creating a
comment on the PR.

Refer to the [GitHub Actions documentation](https://docs.github.com/en/actions) for
more details on adding notifications.

## Change type labels

**Note**: Before using this workflow, ensure that the required labels are created in
your repository settings with these names and the desired colors and descriptions.

I use `#1D76DB` as the label color, but you can use whatever you want.

### `major-change`

Use this label for changes that break compatibility with previous versions, such as
removing a public method, changing a method signature, or modifying the expected
behavior of a method.

* **Recommended GitHub label description**: *The PR introduces changes that could
  break code using this gem*

### `minor-change`

Use this label for changes that add new features, enhance existing features, or
deprecate features in a backward-compatible way, such as adding a new method or
improving performance without breaking existing functionality.

It's also common to include substantial improvements or optimizations in this
category, as long as they don't alter the expected behavior of the existing API.

* **Recommended GitHub label description**: *The PR adds new features, deprecates
  existing features, or makes substantial improvements*

### `patch-change`

Use this label for changes that fix bugs or make other small modifications that do
not affect the API or alter existing functionality, such as fixing user-facing typos
or updating user documentation.

* **Recommended GitHub label description**: *The PR fixes bugs or makes other small
  changes that do not add to or change existing functionality*

### `internal-change`

Use this label for changes that do not impact end-users and do not require a release,
such as updating developer documentation, adjusting CI workflows, or minor code
refactoring.

* **Recommended GitHub label description**: *The PR includes changes that are NOT
  user-facing and will NOT require a release*

### `release`

Use this label when the changes are specifically for creating a new release. Release
PRs should exclusively contain changes necessary for the release, such as version or
CHANGELOG updates. Any unrelated changes should be in separate PRs.

* **Recommended GitHub label description**: *The PR updates files to create a new
  release, such as version files and CHANGELOG files*
