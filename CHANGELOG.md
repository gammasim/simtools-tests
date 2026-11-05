# CHANGELOG

All notable changes to the simtools-tests project will be documented in this file.
Changes for upcoming releases can be found in the [docs/changes](docs/changes) directory.

This changelog is generated using [Towncrier](https://towncrier.readthedocs.io/), see the developer documentation for more information.

<!-- towncrier release notes start -->

## [v5.0.0](https://github.com/gammasim/simtools-tests/releases/tag/v5.0.0) - 2026-11-05

### New Features

- Add integration tests resources for simtools v0.39.0. ([#37](https://github.com/gammasim/simtools-tests/pull/37))
- Add science tests for simtools v0.38.0.
  Compare trigger histograms across baseline and candidate productions.
  Validate the expected change in azimuth angle configuration. ([#38](https://github.com/gammasim/simtools-tests/pull/38))
- Add trigger-histogram related science tests for v0.39.0. ([#40](https://github.com/gammasim/simtools-tests/pull/40))
- Validate science-test template configuration files against simtools schemas in CI. ([#43](https://github.com/gammasim/simtools-tests/pull/43))
- Add telescope-throughput science-test workflows and telescope selections for North and South,
  with comparisons of results from different simtools versions. ([#46](https://github.com/gammasim/simtools-tests/pull/46))
- Add per-site reference-versus-candidate atmosphere comparisons to the shared
  science-test template and v0.39.0 release configuration. North and South
  produce separate atmosphere plots and JSON summaries.
  Atmosphere comparisons now publish candidate-only plots with a warning when baseline ECSV
  files are missing. ([#51](https://github.com/gammasim/simtools-tests/pull/51))
- Add the metadata of two small grid-packed production jobs (job manifests with recorded output
  file sizes and process resource records) as baseline and candidate inputs for the simtools
  computing-comparison integration test. ([#52](https://github.com/gammasim/simtools-tests/pull/52))
- Update `sim_telarray` test files for v0.39.0 including the updated metadata definitions. ([#53](https://github.com/gammasim/simtools-tests/pull/53))
- Update HDF5 histogram objects, which now carry axis names and units. ([#54](https://github.com/gammasim/simtools-tests/pull/54))

### Maintenance

- Add pre-commit setting to prevent submitting files with more than 5,000 lines. ([#42](https://github.com/gammasim/simtools-tests/pull/42))


## [v4.0.0](https://github.com/gammasim/simtools-tests/releases/tag/v4.0.0) - 2026-09-30

### New Features

- Add resource files for simtools v0.37.0. ([#20](https://github.com/gammasim/simtools-tests/pull/20))
- Add resource files for simtools v0.38.0. ([#26](https://github.com/gammasim/simtools-tests/pull/26))
- Test and model parameter updates to reflect usage of Astropy tables for all tabulated simulation model data. ([#28](https://github.com/gammasim/simtools-tests/pull/28))
- Generate nsb and proton hdf5 files for integration tests, and remove overwrite.yaml files. ([#29](https://github.com/gammasim/simtools-tests/pull/29))

### Maintenance

- Consistent simtools installation from GitHub repository. ([#23](https://github.com/gammasim/simtools-tests/pull/23))
- Improve schema validation CI such that it only checks changed files. ([#27](https://github.com/gammasim/simtools-tests/pull/27))
- Remove unused `simtel_config_test_la_palma.cfg` file from static directory for v0.38.0. ([#31](https://github.com/gammasim/simtools-tests/pull/31))
- Update single-pe spectrum test files (related to change in table format in simulation-models). ([#33](https://github.com/gammasim/simtools-tests/pull/33))
- Update `sim_telarray` integration test reference configuration files with updates on `fadc_pulse_shape` versions. ([#34](https://github.com/gammasim/simtools-tests/pull/34))
- Remove obsolete files not required for simtools v0.38.0. ([#35](https://github.com/gammasim/simtools-tests/pull/35))


## [v3.0.0](https://github.com/gammasim/simtools-tests/releases/tag/v3.0.0) - 2026-08-20

### New Features

- Added test and configuration files for simtools v0.35.0. ([#14](https://github.com/gammasim/simtools-tests/pull/14))
- Added resource files for simtools v0.36.0. ([#17](https://github.com/gammasim/simtools-tests/pull/17))

### Maintenance

- Generating test resources for simtools-generate-bias-curve-submissions and
  simtools-simulate-prod-htcondor-generator ([#15](https://github.com/gammasim/simtools-tests/pull/15))


## [v2.0.0](https://github.com/gammasim/simtools-tests/releases/tag/v2.0.0) - 2026-07-15

### New Features

- Add test resources for simtools v0.34.0.
  Following updates with respect to v0.33.0 are applied:
  - run test production with simtools v0.34.0.
  - update `production_configuration` to v1.1.0 and rename `corsika_limits_north.ecsv` to `corsika_limits.ecsv`.
  - update sim_telarray configuration file naming to new shorter file names.
  - apply CLI changes for `simtools-simulate-prod`: rename `pack_for_grid_register` to `grid_output_path`.
  - change default simulation production model version to v7.0.0.
  - remove obsolete static files:
    - `grid_definition.yaml`
    - `production_dl2_fits/prod6_LaPalma-20deg_gamma_cone.N.Am-4LSTs09MSTs_ID0_reduced.fits.gz`
    - `production_dl2_fits/prod6_LaPalma-40deg_gamma_cone.N.Am-4LSTs09MSTs_ID0_reduced.fits.gz`
    - `production_simulation_config_metrics.yml`

  ([#9](https://github.com/gammasim/simtools-tests/pull/9))

### Maintenance

- Fix production configuration to use release version v0.1.0, ensuring test files for v0.33.0 can be downloaded properly. ([#10](https://github.com/gammasim/simtools-tests/pull/10))

## [v1.0.0](https://github.com/gammasim/simtools-tests/releases/tag/v1.0.0) - 2026-06-30

### New Features

- Add resource generation files for v0.33.0. ([#2](https://github.com/gammasim/simtools-tests/pull/2))
- Add resource files for v0.32.0 (legacy files; before addition of the static/generated directory structure). ([#4](https://github.com/gammasim/simtools-tests/pull/4))

### Maintenance

- Use git-lfs for binary files. ([#3](https://github.com/gammasim/simtools-tests/pull/3))
- Improve file format of `download_files.yml` file list. ([#6](https://github.com/gammasim/simtools-tests/pull/6))
