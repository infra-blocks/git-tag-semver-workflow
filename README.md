# git-tag-semver-workflow
[![Release](https://github.com/infra-blocks/git-tag-semver-workflow/actions/workflows/release.yml/badge.svg)](https://github.com/infra-blocks/git-tag-semver-workflow/actions/workflows/release.yml)
[![Update From Template](https://github.com/infra-blocks/git-tag-semver-workflow/actions/workflows/update-from-template.yml/badge.svg)](https://github.com/infra-blocks/git-tag-semver-workflow/actions/workflows/update-from-template.yml)

This reusable workflow manages semantic versioning tags.
See [here](https://github.com/infra-blocks/git-tag-semver-action) for more information on the tagging behavior.

The status of this action is reported as a PR comment using [this action](https://github.com/infra-blocks/status-report-action)
when a matching PR is found for the provided SHA.

## Inputs

|  Name   | Required | Description                                                                                                                                                                                                                                                                                                                                 |
|:-------:|:--------:|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| version |   true   | The semantic version bump to apply. One of "patch", "minor", or "major".                                                                                                                                                                                                                                                                    |
|   sha   |  false   | The commit SHA to tag. Defaults to the ${{ github.sha }}. If the event triggering this workflow is a pull_request, be sure to set this parameter to either ${{ github.event.pull_request.head.sha }} or ${{ github.event.pull_request.base.sha }}. You probably don't want to tag the pull_request default SHA, which is on a merge branch. |
|  skip   |  false   | A boolean indicating whether to skip the workflow. This is to workaround the required checks discrepancy when the workflow is skipped from the caller. It defaults to false.                                                                                                                                                                |

## Secrets

|     Name     | Required | Description                                                                                                                                                                                                                          |
|:------------:|:--------:|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| github-token |  false   | The GitHub token utilized to push the tags. If pushing a tag matching a protection rule, this should be a PAT. Defaults to the $GITHUB_TOKEN otherwise. Note that the workflow still utilizes the $GITHUB_TOKEN to post PR comments. |

## Outputs

| Name | Description                                                |
|:----:|------------------------------------------------------------|
| tags | A stringified JSON array of the triplets of tags upserted. |

## Permissions

|     Scope     | Level | Reason                                        |
|:-------------:|:-----:|-----------------------------------------------|
|   contents    | write | To potentially push tags with this token.     |
| pull-requests | write | To post status report updates as PR comments. |

## Concurrency controls

N/A

## Timeouts

N/A

## Usage

### On push without tag protection rules
```yaml
name: Git Tag Semver

on:
  push: ~

permissions:
  contents: write # Needed for pushing tags
  pull-requests: write # Needed for pushing status reports as PR comments

jobs:
  git-tag-semver:
    uses: infra-blocks/git-tag-semver-workflow/.github/workflows/workflow.yml@v1
    with:
      version: patch
```

### On pull request with tag protection rules
```yaml
name: Git Tag Semver

on:
  pull_request: ~

permissions:
  # Note that we still require those privileges, even though we're not using the
  # GITHUB_TOKEN to push the tags. Permissions are statically defined.
  contents: write
  pull-requests: write # Needed for pushing status reports as PR comments

jobs:
  git-tag-semver:
    uses: infra-blocks/git-tag-semver-workflow/.github/workflows/workflow.yml@v1
    with:
      version: patch
      sha: ${{ github.event.pull_request.head.sha }}
    secrets:
      github-token: ${{ secrets.PAT }}
```

### Releasing

The releasing is handled at git level with semantic versioning tags. Those are automatically generated and managed
by the [git-tag-semver-from-label-workflow](https://github.com/infra-blocks/git-tag-semver-from-label-workflow).
