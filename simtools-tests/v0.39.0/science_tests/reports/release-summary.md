# Science-test summary: v0.39.0

Qualified: False

| Site | Test | Execution | Status | Reason | Report |
| --- | --- | --- | --- | --- | --- |
| north | compare.compute_resources | not_run | not_run | Not selected or not executed. | - |
| north | compare.trigger_histograms | completed | warn | Acceptance warning (29): distribution_core_distance whole_array gamma-e30gev-za60deg-azm31deg-qgs3-ctao-north-alpha.trigger_histograms: value=0.0352 exceeds warning limit 0.03 (PDF page 137). | [report](north/compare.trigger_histograms); [deviations](north/compare.trigger_histograms/acceptance.md) |
| north | derive.trigger_histograms | reused | pass | Reused complete derived products; use --overwrite to rebuild them. | - |
| north | production.gamma.collect | not_run | not_run | Not selected or not executed. | - |
| south | compare.compute_resources | not_run | not_run | Not selected or not executed. | - |
| south | compare.trigger_histograms | completed | warn | Acceptance warning (55): distribution_core_distance whole_array gamma-e30tev-za20deg-azm31deg-qgs3-ctao-south-beta.trigger_histograms: value=0.03098 exceeds warning limit 0.03 (PDF page 5). | [report](south/compare.trigger_histograms); [deviations](south/compare.trigger_histograms/acceptance.md) |
| south | derive.trigger_histograms | reused | pass | Reused complete derived products; use --overwrite to rebuild them. | - |
| south | production.gamma.collect | not_run | not_run | Not selected or not executed. | - |

## Execution details

- north/compare.trigger_histograms: host (no container); [log](/lustre/fs24/group/cta/science_tests/v0.39.0/work/science-tests/north/compare.trigger_histograms/execution.log); work: `/lustre/fs24/group/cta/science_tests/v0.39.0/work/science-tests/north/compare.trigger_histograms`.
- south/compare.trigger_histograms: host (no container); [log](/lustre/fs24/group/cta/science_tests/v0.39.0/work/science-tests/south/compare.trigger_histograms/execution.log); work: `/lustre/fs24/group/cta/science_tests/v0.39.0/work/science-tests/south/compare.trigger_histograms`.

## Production history

- production.gamma.north: submitted (submitted); run ID `01a13bda-daab-71bc-81e9-062d520d3a12`.
- production.gamma.grid.north: completed (pass); run ID `01a13bd3-2977-7212-b5a2-d80f54e722cf`.
- production.gamma.south: submitted (submitted); run ID `01a13bda-daab-71bc-81e9-062d520d3a12`.
- production.gamma.grid.south: completed (pass); run ID `01a13bd3-2977-7212-b5a2-d80f54e722cf`.
