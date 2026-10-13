# Production resource requirements

Valid process records: 1440
Skipped process records: 0

Median and 5th/95th percentiles across jobs, grouped by production and process role.
Hardware and concurrency are not controlled; timing ratios require review.

| Production | Role | Jobs | Metric | Median | P05 | P95 |
| --- | --- | ---: | --- | ---: | ---: | ---: |
| v0.37.0 | corsika | 360 | Wall time (s/event) | 17.0245 | 0.0859056 | 521.541 |
| v0.37.0 | corsika | 360 | CPU time (s/event) | 7.39855 | 0.0714363 | 107.205 |
| v0.37.0 | corsika | 360 | Peak RSS (bytes) | 6.4067e+08 | 5.61942e+08 | 9.65152e+08 |
| v0.37.0 | corsika | 360 | Storage (bytes/event) | - | - | - |
| v0.37.0 | sim_telarray | 360 | Wall time (s/event) | 16.5475 | 0.0818195 | 517.81 |
| v0.37.0 | sim_telarray | 360 | CPU time (s/event) | 3.4765 | 0.000976925 | 9.83668 |
| v0.37.0 | sim_telarray | 360 | Peak RSS (bytes) | 1.91468e+08 | 1.61104e+08 | 4.32854e+08 |
| v0.37.0 | sim_telarray | 360 | Storage (bytes/event) | 3.31105e+06 | 600.168 | 1.42378e+07 |
| v0.38.0_rc1 | corsika | 360 | Wall time (s/event) | 15.0472 | 0.114425 | 545.617 |
| v0.38.0_rc1 | corsika | 360 | CPU time (s/event) | 10.0548 | 0.0836891 | 114.127 |
| v0.38.0_rc1 | corsika | 360 | Peak RSS (bytes) | 6.42118e+08 | 5.62229e+08 | 9.66672e+08 |
| v0.38.0_rc1 | corsika | 360 | Storage (bytes/event) | - | - | - |
| v0.38.0_rc1 | sim_telarray | 360 | Wall time (s/event) | 14.6837 | 0.110166 | 541.117 |
| v0.38.0_rc1 | sim_telarray | 360 | CPU time (s/event) | 3.94468 | 0.000698275 | 11.758 |
| v0.38.0_rc1 | sim_telarray | 360 | Peak RSS (bytes) | 1.92668e+08 | 1.58297e+08 | 4.25379e+08 |
| v0.38.0_rc1 | sim_telarray | 360 | Storage (bytes/event) | 3.18835e+06 | 529.812 | 1.40448e+07 |
