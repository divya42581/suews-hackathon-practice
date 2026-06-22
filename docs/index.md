# SUEWS Hackathon Practice Setup

This is a practice repository for the SUEWS Community Hackathon setup workflow.

## Setup status

- Repository created from `UMEP-dev/suews-hackathon-template`.
- `TASK_BRIEF.md` reviewed.
- SUEWS runtime installed in a local Python environment.
- One bundled `simple-urban` demo case initialised, validated, run, diagnosed,
  and summarised.
- GitHub Pages configured to publish this `docs/` folder.

## Smoke-test run

The practice run is saved in `analysis/demo-simple-urban/`.

- Output file: `analysis/demo-simple-urban/Output/KCL1_2012_SUEWS_60.txt`
- Summary check: `QH`, `QE`, and `QN` all had 0% NaN values.
- Diagnostics: 3 pass, 1 warning, 0 fail.

## Interpretation boundary

This is only a toolchain smoke test. It uses the packaged KCL/London sample
configuration and forcing data, so it confirms that the local SUEWS workflow can
run end to end but does not represent the hackathon focus city or a
site-specific heat-risk result.

## Next hackathon step

On the day, replace the sample configuration and forcing with the released focus
city dataset, run the selected scenarios through the suews-agent, apply the
provided heat-to-risk bridge, and update this page with the final public
submission narrative.
