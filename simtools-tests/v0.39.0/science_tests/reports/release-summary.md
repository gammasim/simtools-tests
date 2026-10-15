# Science-test summary: v0.39.0

Qualified: False

| Site | Test | Execution | Status | Reason | Report |
| --- | --- | --- | --- | --- | --- |
| north | compare.compute_resources | not_run | not_run | See excerpt from log file above | - |
| north | compare.trigger_histograms | completed | warn | Numerical acceptance is advisory; review required. | [report](north/compare.trigger_histograms) |
| north | derive.trigger_histograms | reused | pass | Reused complete derived products; use --overwrite to rebuild them. | - |
| north | production.gamma.collect | completed | pass | - | - |
| south | compare.compute_resources | not_run | not_run | See excerpt from log file above | - |
| south | compare.trigger_histograms | completed | warn | Numerical acceptance is advisory; review required. | [report](south/compare.trigger_histograms) |
| south | derive.trigger_histograms | reused | pass | Reused complete derived products; use --overwrite to rebuild them. | - |
| south | production.gamma.collect | completed | pass | - | - |

## Execution details

- north/compare.compute_resources: host (no container); [log](/lustre/fs24/group/cta/science_tests/v0.39.0/work/science-tests/north/compare.compute_resources/execution.log); work: `/lustre/fs24/group/cta/science_tests/v0.39.0/work/science-tests/north/compare.compute_resources`.
- north/compare.trigger_histograms: host (no container); [log](/lustre/fs24/group/cta/science_tests/v0.39.0/work/science-tests/north/compare.trigger_histograms/execution.log); work: `/lustre/fs24/group/cta/science_tests/v0.39.0/work/science-tests/north/compare.trigger_histograms`.
- south/compare.compute_resources: host (no container); [log](/lustre/fs24/group/cta/science_tests/v0.39.0/work/science-tests/south/compare.compute_resources/execution.log); work: `/lustre/fs24/group/cta/science_tests/v0.39.0/work/science-tests/south/compare.compute_resources`.
- south/compare.trigger_histograms: host (no container); [log](/lustre/fs24/group/cta/science_tests/v0.39.0/work/science-tests/south/compare.trigger_histograms/execution.log); work: `/lustre/fs24/group/cta/science_tests/v0.39.0/work/science-tests/south/compare.trigger_histograms`.

## Production history

- production.gamma.north: submitted (submitted); run ID `01a13bda-daab-71bc-81e9-062d520d3a12`.
- production.gamma.grid.north: completed (pass); run ID `01a13bd3-2977-7212-b5a2-d80f54e722cf`.
- production.gamma.south: submitted (submitted); run ID `01a13bda-daab-71bc-81e9-062d520d3a12`.
- production.gamma.grid.south: completed (pass); run ID `01a13bd3-2977-7212-b5a2-d80f54e722cf`.
