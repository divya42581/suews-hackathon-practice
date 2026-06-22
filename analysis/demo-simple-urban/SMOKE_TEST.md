# SUEWS smoke test

Date: 2026-06-22

This folder contains a small end-to-end SUEWS demo run created with the bundled
`simple-urban` template.

## Commands run

```powershell
.venv\Scripts\suews.exe init --template simple-urban --format json analysis\demo-simple-urban
.venv\Scripts\suews.exe validate --format json analysis\demo-simple-urban\sample_config.yml
.venv\Scripts\suews.exe run analysis\demo-simple-urban\updated_sample_config.yml
.venv\Scripts\suews.exe diagnose --format json analysis\demo-simple-urban
.venv\Scripts\suews.exe summarise --format json --variables QH,QE,QN analysis\demo-simple-urban\Output
```

## Result

- Run status: completed.
- Output file: `Output/KCL1_2012_SUEWS_60.txt`.
- Summary check: `QH`, `QE`, and `QN` all had 0% NaN values.
- Diagnostics: 2 pass, 2 warning, 0 fail.

## Boundary

This is a demo readiness run using the packaged KCL/London sample forcing and
configuration. It confirms the local SUEWS toolchain can initialise, validate,
run, diagnose, and summarise a case; it should not be interpreted as the
hackathon city analysis.
