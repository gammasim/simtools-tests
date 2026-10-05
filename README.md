# simtools-tests

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.21070798.svg)](https://doi.org/10.5281/zenodo.21070798)

Versioned test resources and generated artifacts for
[simtools](https://github.com/gammasim/simtools) integration and science-validation workflows.

## Integration tests

Current bundles live below `simtools-tests/<resource-version>/integration_tests/`. Bundles below
`simtools-tests/legacy/` are not maintained by current CI.

| Path | Lifecycle |
| --- | --- |
| `config_files/` | Versioned workflow inputs. |
| `static/` | Hand-maintained inputs; update `static/static_manifest.yml` with every change. |
| `downloaded/` | Inputs fetched from `config_files/download_files.yml`; regenerated from external GitLab URLs. |
| `generated/` | Workflow outputs collected as reference products. |
| `run_time.yml` | Container/runtime image, mounts, network, and environment-file configuration. |

Science-test releases, when present, live in `<resource-version>/science_tests/` and contain the
release definition, site selections, context example, and runner instructions for
`simtools-run-science-tests`.

Do not commit `tmp/`, `tmp_application_output/`, or `log_files/`; the other directories are part of
the versioned snapshot.

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

## Run simtools integration tests

From the `simtools` repository root, select a resource directory with
`--test_resources_path`:

```bash
resource_version="vX.Y.Z"
pytest --no-cov -n auto --model_version=6.0.2 \
    --test_resources_path="../simtools-tests/simtools-tests/${resource_version}/integration_tests" \
    tests/integration_tests
```

Alternatively, select a versioned resource directory with
`--simtools_tests_resource_version` (or `SIMTOOLS_TESTS_RESOURCE_VERSION`); the default is the
`resource-version` in `simtools/dependency_versions.yml`. A full resource path can also be supplied
through `SIMTOOLS_TEST_RESOURCES`.

When using a local model checkout, set `SIMTOOLS_SIMULATION_MODELS_PATH`. For a Git source, set
`SIMTOOLS_SIMULATION_MODELS_GIT_PATH` and `SIMTOOLS_SIMULATION_MODELS_GIT_REVISION` instead; the two
source types cannot be used together. This assumes sibling `simtools` and `simtools-tests` checkouts.
See
[CONTRIBUTING.md](https://github.com/gammasim/simtools/blob/main/CONTRIBUTING.md) and the
[RELEASING.md](https://github.com/gammasim/simtools/blob/main/docs/source/developer-guide/release.md)
release guidance for project workflow.

## Science tests

### Science-test template

The shared `science-test-template/` directory contains reusable science-test definitions. Release
bundles refer to this template instead of copying workflows.

| Path | Purpose |
| --- | --- |
| `catalogue.yml` | Test names, supported sites, prerequisite tests, and production requirements. |
| `workflows/` | Production, derivation, and comparison workflows. |
| `profiles/` | Shared local and HTCondor execution settings. |
| `acceptance/expected-products.yml` | Required products for every test. |
| `acceptance/thresholds.yml` | Comparison limits and checks for complete simulation outputs. |

The template uses the existing simtools application-workflow schema. Production is always an
explicit, two-step operation: review the generated grid first, then submit with
`--allow_production`. Comparison tests never submit production implicitly. Numerical comparison
results remain advisory until calibrated thresholds are approved.

### Run simtools science tests

Science tests are longer-running release-validation workflows. A release directory contains a
release definition and site selections. Copy its context example outside the repository, fill in
the candidate, baseline, and production-configuration directories, and check the configuration
with a dry run.

Set `__SCIENCE_CONTAINER_IMAGE_PATH__` to the full path of the Apptainer `.sif` file for
HTCondor production. The file can have any name and can be stored outside the candidate directory.

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

After production completes, run the requested derivation and comparison tests without resubmitting:

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
