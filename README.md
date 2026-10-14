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
All paths must be readable from both the submit host and the HTCondor worker inside Apptainer:

| Context entry | Shared location |
| --- | --- |
| `simulation_models_path` or `simulation_models_git_path` | `/lustre/fs24/group/cta/science_tests/simulation-models` |
| `__CORSIKA_INTERACTION_TABLE_PATH__` | `/lustre/fs24/group/cta/science_tests/corsika7-interaction-tables/interaction-tables` |
| `__PRODUCTION_CONFIGURATION_PATH__` | `/lustre/fs24/group/cta/science_tests/production-configuration/data` |

The release context also requires the candidate/baseline directories and the versioned Apptainer image.
Avoid personal `/cephfs/users/...` paths in shared release configurations. Run the recommended
`--preflight` before production submission; it validates these paths on the worker before submitting
the production job set.

### Examples for science tests

```bash
simtools-run-science-tests \
    --release_dir ../simtools-tests/simtools-tests/v0.38.0/science_tests \
    --test compare.compute_resources \
    --site North
```
