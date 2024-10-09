<!--
# @markup markdown
# @title How To Contribute
-->

# Contributing to this project

* [Summary](#summary)
* [How to contribute](#how-to-contribute)
* [How to report an issue or request a feature](#how-to-report-an-issue-or-request-a-feature)
* [How to submit a code or documentation change](#how-to-submit-a-code-or-documentation-change)
* [Design philosophy](#design-philosophy)
* [Coding standards](#coding-standards)
  * [Commit message guidelines](#commit-message-guidelines)
  * [Pull request guidelines](#pull-request-guidelines)
* [Licensing](#licensing)

## Summary

...

## How to contribute

...

## How to report an issue or request a feature

...

## How to submit a code or documentation change

...

## Design philosophy

...

## Coding standards

### Commit message guidelines

All commit messages must follow the [Conventional Commits
standard](https://www.conventionalcommits.org/en/v1.0.0/). This helps us maintain a
clear and structured commit history, automate versioning, and generate changelogs
effectively.

To ensure compliance, this project includes:

* A git commit-msg hook that validates your commit messages before they are accepted.

  To activate the hook, you must have node installed and run `npm install`.

* A GitHub Actions workflow that will enforce the Conventional Commit standard as
  part of the continuous integration pipeline.

  Any commit message that does not conform to the Conventional Commits standard will
  cause the workflow to fail and not allow the PR to be merged.

### Pull request guidelines

All pull requests must be merged using rebase merges. This ensures that commit
messages from the feature branch are preserved in the release branch, keeping the
history clean and meaningful.

## Licensing

...
