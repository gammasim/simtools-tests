# simtools-tests

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.21070798.svg)](https://doi.org/10.5281/zenodo.21070798)

Versioned test resources and generated artifacts for
[simtools](https://github.com/gammasim/simtools) integration and science-validation workflows.

## Resource bundles

Current bundles live below `simtools-tests/<resource-version>/integration_tests/`. Bundles below
`simtools-tests/legacy/` are historical and are not maintained by current CI.

| Path | Lifecycle |
| --- | --- |
| `config_files/` | Versioned workflow inputs. |
| `static/` | Hand-maintained inputs; update `static/static_manifest.yml` with every change. |
| `downloaded/` | Inputs fetched from `config_files/download_files.yml`; regenerated from external GitLab URLs. |
| `generated/` | Workflow outputs collected as reference products. |
| `log_files/` | One generation log per workflow. |
| `tmp/` | Disposable per-application scratch data. |
| `tmp_application_output/` | Disposable staging output before collection. |
| `run_time.yml` | Container/runtime image, mounts, network, and environment-file configuration. |

Do not commit `tmp/` or `tmp_application_output/`; the other directories are part of the versioned
snapshot.

## Generation requirements

Run generation from this repository root with the already configured `simtools` environment. The
runtime in `run_time.yml` requires Podman or Docker and access to the configured image registry.
Generation also needs network access to the GitLab URLs in `download_files.yml` and, for database
workflows, MongoDB. The local `.env` must provide the database settings
(`SIMTOOLS_DB_SERVER`, `SIMTOOLS_DB_API_PORT`, `SIMTOOLS_DB_API_USER`, `SIMTOOLS_DB_API_PW`,
`SIMTOOLS_DB_API_AUTHENTICATION_DATABASE`, and `SIMTOOLS_DB_SIMULATION_MODEL[_TAG]`) and the
container paths `SIMTOOLS_SIM_TELARRAY_PATH`, `SIMTOOLS_CORSIKA_PATH`, and
`SIMTOOLS_CORSIKA_INTERACTION_TABLE_PATH`. Keep `.env` out of commits.

## Generate or regenerate

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

## Verify and compare

`--test_static_files` verifies the static manifest. Verify all maintained bundles as CI does:

```bash
for integration_tests in simtools-tests/v*/integration_tests; do
    resource_version="${integration_tests#simtools-tests/}"
    resource_version="${resource_version%/integration_tests}"
    simtools-resources-test-generate \
        --test_directory . \
        --simtools_version "${resource_version}" \
        --test_static_files
done
```

Start with a clean working tree, then compare a deterministic regeneration with:

```bash
resource_version="vX.Y.Z"
git status --short
git diff --check
git diff --exit-code -- "simtools-tests/${resource_version}/integration_tests"
```

For a new bundle, review the untracked files with `git status --short` before adding them. Remove
only disposable output after inspection:

```bash
resource_version="vX.Y.Z"
rm -rf "simtools-tests/${resource_version}/integration_tests/tmp" \
       "simtools-tests/${resource_version}/integration_tests/tmp_application_output"
```

## Run simtools integration tests

From the `simtools` repository root, select resources with the underscore option registered by
`tests/conftest.py`:

```bash
resource_version="vX.Y.Z"
pytest --no-cov -n auto --model_version=6.0.2 \
    --test_resources_path="../simtools-tests/simtools-tests/${resource_version}/integration_tests" \
    tests/integration_tests
```

This assumes sibling `simtools` and `simtools-tests` checkouts. See
[CONTRIBUTING.md](https://github.com/gammasim/simtools/blob/main/CONTRIBUTING.md) and the
[RELEASING.md](https://github.com/gammasim/simtools/blob/main/docs/source/developer-guide/release.md)
release guidance for project workflow.
