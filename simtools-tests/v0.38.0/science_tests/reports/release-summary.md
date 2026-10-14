# Science-test summary: v0.38.0

Qualified: False

| Site | Test | Execution | Status | Reason | Report |
| --- | --- | --- | --- | --- | --- |
| north | compare.compute_resources | not_run | not_run | Not selected or not executed. | - |
| north | compare.trigger_histograms | not_run | not_run | Not selected or not executed. | - |
| north | derive.trigger_histograms | completed | pass | - | [report](north/derive.trigger_histograms) |
| north | production.gamma.collect | not_run | not_run | Not selected or not executed. | - |
| south | compare.compute_resources | not_run | not_run | Not selected or not executed. | - |
| south | compare.trigger_histograms | not_run | not_run | Not selected or not executed. | - |
| south | derive.trigger_histograms | failed | incomplete | See excerpt from log file above | - |
| south | production.gamma.collect | not_run | not_run | Not selected or not executed. | - |

## Execution details

- north/derive.trigger_histograms: apptainer (image: /lustre/fs24/group/cta/science_tests/v0.38.0/simtools-prod_20261006-v78050-v2025-11-30-rc-avx2.sif); [log](/lustre/fs24/group/cta/science_tests/v0.38.0/work/science-tests/north/derive.trigger_histograms/execution.log); work: `/lustre/fs24/group/cta/science_tests/v0.38.0/work/science-tests/north/derive.trigger_histograms`.
- south/derive.trigger_histograms: apptainer (image: /lustre/fs24/group/cta/science_tests/v0.38.0/simtools-prod_20261006-v78050-v2025-11-30-rc-avx2.sif); [log](/lustre/fs24/group/cta/science_tests/v0.38.0/work/science-tests/south/derive.trigger_histograms/execution.log); work: `/lustre/fs24/group/cta/science_tests/v0.38.0/work/science-tests/south/derive.trigger_histograms`.

## Production history

- production.gamma.north: submitted (submitted); run ID `01a1368d-f191-7362-8474-592314cba19a`.
- production.gamma.grid.north: completed (pass); run ID `01a1359a-5f3e-70e1-8e0a-4fd6f03dad73`.
- production.gamma.south: submitted (submitted); run ID `01a1368d-f191-7362-8474-592314cba19a`.
- production.gamma.grid.south: completed (pass); run ID `01a1359a-5f3e-70e1-8e0a-4fd6f03dad73`.
