# CHANGELOG

All notable changes to the simtools-tests project will be documented in this file.
Changes for upcoming releases can be found in the [docs/changes](docs/changes) directory.

This changelog is generated using [Towncrier](https://towncrier.readthedocs.io/), see the developer documentation for more information.

<!-- towncrier release notes start -->

## [v2.0.0](https://github.com/gammasim/simtools-tests/releases/tag/v2.0.0) - 2026-07-15

### New Features

- Add test resources for simtools v0.34.0.
  Following updates with respect to v0.33.0 are applied:
  - run test production with simtools v0.34.0.
  - update `production_configuration` to v1.1.0 and rename `corsika_limits_north.ecsv` to `corsika_limits.ecsv`.
  - update sim_telarray configuration file naming to new shorter file names.
  - apply changes in CL for `simtools-simulate-prod` from `pack_for_grid_register` to `grid_output_path`.
  - changed default simulation production model version to v7.0.0.
  - remove obsolete static files grid_definition.yaml, production_dl2_fits/prod6_LaPalma-20deg_gamma_cone.N.Am-4LSTs09MSTs_ID0_reduced.fits.gz, production_dl2_fits/prod6_LaPalma-40deg_gamma_cone.N.Am-4LSTs09MSTs_ID0_reduced.fits.gz, production_simulation_config_metrics.yml

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
