# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [2.1.2] - 2024-05-18

### Changed

- Migrated to the [infra-blocks](https://github.com/infra-blocks) organization.

## [2.1.1] - 2024-05-02

### Fixed

- Use `inputs.skip` at step level, and not at job level. Skipping at job level results in the same issue with
required checks paths as skipping at the caller's workflow invocation site (with an `if` expression).

## [2.1.0] - 2024-04-20

### Added

- The `skip` input to skip on the callee's side of a `worfklow_call`. Otherwise, when the workflow is skipped from
  the caller's side (with an `if` expression), the check path when the workflow was run is not the same as when
  it was skipped ( `outer / inner` vs. `outer` when skipped) !


## [2.0.1] - 2024-03-17

### Changed

- Updated CI dependency to `check-has-semver-label-workflow@v2`
- Updated CI dependency to `git-tag-semver-from-label-workflow@v2`
- Updated CI dependency to `merge-template-workflow@v2`
- Use `get-current-pull-request-action` internally instead of `8BitJonny/gh-get-current-pr`

### Added

- Status badges in README.

## [2.0.0] - 2024-01-19

### Removed

- The legacy file exporting the workflow. Now the workflow is only accessible at `.github/workflows/workflow.yml`.

## [1.1.0] - 2024-01-18

### Added

- A duplicate of the reusable workflow at the `.github/workflows/workflow.yml`. This is part of a change to have
  every reusable workflow exported under the same conventional path.

## [1.0.1] - 2024-01-06

### Fixed

- Post the status report to the PR related to `inputs.sha`, and not the current PR. Those could be different.

## [1.0.0] - 2024-01-05

### Added

- First release!

[2.1.2]: https://github.com/infra-blocks/git-tag-semver-workflow/compare/v2.1.1...v2.1.2
[2.1.1]: https://github.com/infra-blocks/git-tag-semver-workflow/compare/v2.1.0...v2.1.1
[2.1.0]: https://github.com/infra-blocks/git-tag-semver-workflow/compare/v2.0.1...v2.1.0
[2.0.1]: https://github.com/infra-blocks/git-tag-semver-workflow/compare/v2.0.0...v2.0.1
[2.0.0]: https://github.com/infra-blocks/git-tag-semver-workflow/compare/v1.1.0...v2.0.0
[1.1.0]: https://github.com/infra-blocks/git-tag-semver-workflow/compare/v1.0.1...v1.1.0
[1.0.1]: https://github.com/infra-blocks/git-tag-semver-workflow/compare/v1.0.0...v1.0.1
[1.0.0]: https://github.com/infra-blocks/git-tag-semver-workflow/releases/tag/v1.0.0
