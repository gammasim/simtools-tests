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

For a new bundle, replace the version values below. The target must not already exist; the template
copies `config_files/`, `static/`, and `run_time.yml`, then generation creates `downloaded/`,
`generated/`, and `log_files/`.

```bash
resource_version="vX.Y.Z"
template_version="vA.B.C"
runtime_file="simtools-tests/${template_version}/integration_tests/run_time.yml"
test ! -e "simtools-tests/${resource_version}"
simtools-resources-test-generate \
    --simtools_version "${resource_version}" \
    --template_version "${template_version}" \
    --test_directory . \
    --runtime_environment_file "${runtime_file}" \
    --overwrite_collection_files
```

For an existing bundle, review its `run_time.yml` first and rerun with:

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

### Science-test template

The shared `science-test-template/` directory contains reusable science-test definitions. Release
bundles refer to this template instead of copying workflows.

| Path | Purpose |
| --- | --- |
| `catalogue.yml` | Test names, supported sites, prerequisite tests, and production requirements. |
| `workflows/` | Production, derivation, and comparison workflows. |
| `run_time.yml` | Shared Apptainer runtime for grid generation, derivation, and comparison. |
| `profiles/` | Shared local and HTCondor execution settings. |
| `acceptance/expected-products.yml` | Required products for every test. |
| `acceptance/thresholds.yml` | Comparison limits and checks for complete simulation outputs. |

The template uses the simtools application-workflow schema. Production is always an
explicit, two-step operation: review the generated grid first, then submit with
`--allow_production`. Comparison tests never submit production implicitly. Numerical comparison
results remain advisory until calibrated thresholds are approved.

### Run simtools science tests

Science tests are longer-running release-validation workflows. A release directory contains a
release definition and site selections. Copy its context example, fill in
the candidate, baseline, and production-configuration directories, and check the configuration
with a dry run.

Set `__SCIENCE_CONTAINER_IMAGE_PATH__` to the full path of the Apptainer `.sif` file for
the shared runtime and HTCondor production. The file can have any name and can be stored outside
the candidate directory. The science runner loads `science-test-template/run_time.yml` automatically;
`__CONFIG_DIRECTORY__` in that file refers to the template directory. Production submission runs
on the host using `profiles/htcondor.yml`; the submitted jobs use its container image.

```bash
simtools-run-science-tests \
    --release_dir simtools-tests/vX.Y.Z/science_tests \
    --context_file /path/to/vX.Y.Z-context.yml \
    --dry_run
```

Generate and review the production grids before submitting:

```bash
simtools-run-science-tests \
    --release_dir simtools-tests/vX.Y.Z/science_tests \
    --context_file /path/to/vX.Y.Z-context.yml \
    --test production.gamma.grid

simtools-run-science-tests \
    --release_dir simtools-tests/vX.Y.Z/science_tests \
    --context_file /path/to/vX.Y.Z-context.yml \
    --test production.gamma \
    --allow_production
```

Submission returns once jobs are queued. Use `--test production.gamma.collect` to check completion;
collection returns `pending` while jobs remain queued. Comparison tests collect production
automatically before running:

```bash
simtools-run-science-tests \
    --release_dir simtools-tests/vX.Y.Z/science_tests \
    --context_file /path/to/vX.Y.Z-context.yml \
    --test compare.trigger_histograms \
    --test compare.compute_resources
```

Repeat `--site` to select sites. Dry runs do not write reports; running selected tests retains
the summary for all required tests at each site. Reports are prepared in the candidate directory
and copied into the release directory with file paths and checksums.
