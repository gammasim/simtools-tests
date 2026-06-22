# simtools-tests

This repository stores versioned test resources and generated artifacts used by
simtools integration tests, compatibility checks, and science validation
workflows.

The main `simtools` repository should keep the current lightweight test
resources needed for pull-request CI. This repository is intended for larger,
frozen, or release-specific resource sets that should be preserved separately.

## Directory structure

Generated test resources are stored in version directories directly below
`simtools-tests/`, for example `simtools-tests/v0.33.0/`. These directories are
validated by CI and must follow the current generated-resource structure.

Historical resources that predate this structure are stored below
`simtools-tests/legacy/`. They are preserved for reference and compatibility,
but are not generated, validated, linted, or otherwise supported by the current
CI workflows.
