# git-tag-semver-workflow

This reusable workflow manages semantic versioning tags.
See [here](https://github.com/infrastructure-blocks/git-tag-semver-action) for more information on the tagging behavior.

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
    uses: infrastructure-blocks/git-tag-semver-workflow/.github/workflows/git-tag-semver.yml@v1
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
    uses: infrastructure-blocks/git-tag-semver-workflow/.github/workflows/git-tag-semver.yml@v1
    with:
      version: patch
      sha: ${{ github.event.pull_request.head.sha }}
    secrets:
      github-token: ${{ secrets.PAT }}
```

### Releasing

The releasing is handled at git level with semantic versioning tags. Those are automatically generated and managed
by the [git-tag-semver-from-label-workflow](https://github.com/infrastructure-blocks/git-tag-semver-from-label-workflow).
