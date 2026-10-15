# simtools-tests

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.21070798.svg)](https://doi.org/10.5281/zenodo.21070798)

Versioned test resources and generated artifacts for
[simtools](https://github.com/gammasim/simtools) integration and science-validation workflows.

## Integration tests

Directory for integration tests is `simtools-tests/<resource-version>/integration_tests/`. Files below
`simtools-tests/legacy/` are not maintained by current CI.

| Path | Lifecycle |
| --- | --- |
| `config_files/` | Versioned workflow inputs. |
| `static/` | Hand-maintained inputs; update `static/static_manifest.yml` with every change. |
| `downloaded/` | Inputs fetched from `config_files/download_files.yml`; regenerated from external GitLab URLs. |
| `generated/` | Workflow outputs collected as reference products. |
| `run_time.yml` | Container/runtime image, mounts, network, and environment-file configuration. |

### Generation requirements

Run generation from this repository root with the already configured `simtools` environment. The
runtime in `run_time.yml` requires Podman or Docker and access to the configured image registry.
Generation also needs network access to the GitLab URLs in `download_files.yml`. Simulation models
are read from a filesystem repository or a local Git repository. Copy `../simtools/.env_template`
to `.env` and set the container paths `SIMTOOLS_SIM_TELARRAY_PATH`, `SIMTOOLS_CORSIKA_PATH`, and
`SIMTOOLS_CORSIKA_INTERACTION_TABLE_PATH`, plus one of:

- `SIMTOOLS_SIMULATION_MODELS_PATH` for a checked-out simulation-model repository; or
- `SIMTOOLS_SIMULATION_MODELS_GIT_PATH` and `SIMTOOLS_SIMULATION_MODELS_GIT_REVISION` for a local
  Git repository.

Set `SIMTOOLS_TESTS_PATH` when the container needs to resolve versioned resources from this
checkout. Keep `.env` out of commits.

### Generate or regenerate

For a new resource set, replace the version values below. The target must not already exist; the template
copies `config_files/`, `static/`, and `run_time.yml`, then generation creates `downloaded/`,
`generated/`, and `log_files/`.

```bash
resource_version="vX.Y.Z"
template_version="vA.B.C"
runtime_file="simtools-tests/${template_version}/integration_tests/run_time.yml"
simtools-resources-test-generate \
    --simtools_version "${resource_version}" \
    --template_version "${template_version}" \
    --test_directory . \
    --runtime_environment_file "${runtime_file}" \
    --overwrite_collection_files
```

For an existing resource set, review its `run_time.yml` first and rerun with:

```bash
resource_version="vX.Y.Z"
simtools-resources-test-generate \
    --simtools_version "${resource_version}" \
    --test_directory . \
    --runtime_environment_file "simtools-tests/${resource_version}/integration_tests/run_time.yml" \
    --overwrite_collection_files
```

A successful run exits 0, writes one log per workflow, and updates the collected outputs. Use
`--config_file path/to/workflow.config.yml` to run one workflow or `--download_only` to fetch only
external inputs.

## Science tests

This repository owns the shared `science-test-template/` resources and versioned
`simtools-tests/<version>/science_tests/` release configurations and reports. The `simtools`
repository owns the runner and its operating instructions. See the [science-test guide in
simtools](https://github.com/gammasim/simtools/blob/main/docs/source/developer-guide/testing_science.md)
for setup, execution, and result interpretation.

### Science-test dependencies

Before running a release, configure the external paths in that release's `science_tests/context.yml`.
Set the shared storage root locally, using an absolute path:

```bash
export SIMTOOLS_SCIENCE_TESTS_ROOT=/path/to/shared/science_tests
```

Alternatively, put `SIMTOOLS_SCIENCE_TESTS_ROOT=/path/to/shared/science_tests` in an untracked `.env`
in the working directory, or in a file selected with `--env_file`. Context paths can then use
`${SIMTOOLS_SCIENCE_TESTS_ROOT}`, for example:

```yaml
__SCIENCE_CANDIDATE_PATH__: ${SIMTOOLS_SCIENCE_TESTS_ROOT}/v0.39.0
__SCIENCE_BASELINE_PATH__: ${SIMTOOLS_SCIENCE_TESTS_ROOT}/v0.37.0
```

The runner resolves the root before validating paths and preparing workflows, and rejects an
unset, empty, or relative root when the context references it. Explicit absolute context paths
remain supported. Keep filesystem-specific roots in local configuration. The shared dependencies
use these locations below the root:

| Context entry | Location below the root |
| --- | --- |
| `simulation_models_path` or `simulation_models_git_path` | `simulation-models` |
| `__CORSIKA_INTERACTION_TABLE_PATH__` | `corsika7-interaction-tables/interaction-tables` |
| `__PRODUCTION_CONFIGURATION_PATH__` | `production-configuration/data` |

The release context also requires the candidate/baseline directories and the versioned Apptainer image.
HTCondor workflows share their job settings through `science-test-template/profiles/htcondor.yml`.
The worker environment profile derives `SIMTOOLS_CORSIKA_INTERACTION_TABLE_PATH` from
`SIMTOOLS_SCIENCE_TESTS_ROOT`, so keep the matching `corsika7-interaction-tables/interaction-tables`
directory below that root. The HTCondor submit environment expands the variable before creating the
worker bind.
All resolved paths must be readable at the same location on the submit host and the HTCondor worker
inside Apptainer. Run the recommended `--preflight` before production submission; it validates these
paths on the worker before submitting the production job set.

### Examples for science tests

Create a release configuration from the shared templates, then edit `context.yml` with the
candidate, baseline (for comparison tests), production-configuration, simulation-model, and CORSIKA
paths required by your environment:

```bash
release_dir="simtools-tests/vX.Y.Z/science_tests"
simtools-run-science-tests --release_dir "${release_dir}" --setup \
    --baseline_version vA.B.C --simulation_models_git_revision v0.18.0
```

The optional version arguments set the baseline and model Git ref in `release.yml`, and the
baseline directory in `context.yml`. Review the generated release and context settings before running.

Validate the configuration without running workflows, then select a test. For example, compare
compute resources for the North site:

```bash
simtools-run-science-tests --release_dir "${release_dir}" --dry_run
simtools-run-science-tests \
    --release_dir "${release_dir}" \
    --test compare.compute_resources \
    --site North
```

For production tests, review the generated grid and run `--preflight` before submission; production
also requires `--allow_production`. See the [science-test guide in simtools](https://github.com/gammasim/simtools/blob/main/docs/source/developer-guide/testing_science.md)
for the full workflow and result interpretation.
