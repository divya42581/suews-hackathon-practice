# SUEWS smoke test

This directory contains a small `simple-urban` example run created to confirm that the SUEWS agent/runtime works end to end on this machine.

What worked:

- `suews init` created `sample_config.yml` and `Kc_2012_data_60.txt`.
- `suews validate` completed with warnings but no errors.
- `suews run analysis/example-run/updated_sample_config.yml` completed successfully.
- SUEWS wrote `Output/KCL1_2012_SUEWS_60.txt` and `Output/KCL_SUEWS_checkpoint.json`.
- `suews summarise` reported 8784 time steps and 0.0% NaNs for key variables including `QN`, `QH`, `QE`, `T2`, and `RH2`.

Caveat: this is the packaged demo case, not a calibrated hackathon submission. Diagnostics reported warnings for missing provenance before this file was added and for energy-balance closure in the demo output.
