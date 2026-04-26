# anombyte93 / .github

Account-wide default PR contract and reusable GitHub workflows for `anombyte93` repositories.

## What this repo does

- provides the default pull request template for repositories that do not define their own `pull_request_template.md`
- hosts the reusable `PR Evidence Contract` workflow that validates PR bodies, comments on missing context, and converts noncompliant PRs back to draft

## Default PR template

File: [`pull_request_template.md`](pull_request_template.md)

GitHub uses this template for any `anombyte93` repository that does not override it locally. The template requires:

- plain-English summary and why
- discovery context
- architecture decision
- evidence and references
- explicit doubts, risks, and critic
- requested feedback

## Reusable PR enforcement workflow

File: [`.github/workflows/pr-evidence-contract.yml`](.github/workflows/pr-evidence-contract.yml)

To enforce the contract in a repository, add this caller workflow:

```yaml
name: PR Evidence Contract

on:
  pull_request_target:
    types: [opened, edited, synchronize, reopened, ready_for_review]

permissions:
  contents: read
  issues: write
  pull-requests: write

jobs:
  pr-evidence-contract:
    uses: anombyte93/.github/.github/workflows/pr-evidence-contract.yml@main
    permissions:
      contents: read
      issues: write
      pull-requests: write
```

## Why `pull_request_target`

The enforcement workflow never checks out PR-head code. It only reads the PR body and uses the trusted base-branch workflow definition to:

- validate required sections
- upsert or remove a bot comment
- fail the run if the contract is broken
- convert the PR back to draft when it is not merge-ready

That is the strongest action-level guard available without private-repo rulesets or required status checks.
Account-wide default PR templates and community health files for anombyte93 repositories
