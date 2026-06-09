# SUEWS hackathon practice setup transcript

Date: 2026-06-09

## User request

Set up a public GitHub practice repository from `UMEP-dev/suews-hackathon-template`, read the task brief, run a small SUEWS simulation using the SUEWS agent/runtime, publish `docs/` via GitHub Pages, save this transcript, then commit and push.

## Setup notes

- GitHub account confirmed as `divya42581`.
- `gh` and `git` were installed but not on `PATH`; used full executable paths:
  - `C:\Program Files\GitHub CLI\gh.exe`
  - `C:\Program Files\Git\cmd\git.exe`
- Initial `gh auth status` showed an invalid keyring token. The user refreshed GitHub CLI authentication and replied `DONE`.
- After refresh, `gh auth status` succeeded for `divya42581`.

## Repository

- Target repo: `divya42581/suews-hackathon-practice`
- Repo URL: <https://github.com/divya42581/suews-hackathon-practice>
- Visibility: public
- The repo already existed when checked, so it was cloned locally:

```powershell
& 'C:\Program Files\Git\cmd\git.exe' clone https://github.com/divya42581/suews-hackathon-practice.git suews-hackathon-practice
```

- The repository was opened in the default browser.

## Task brief

Read `TASK_BRIEF.md`. Summary:

- The hackathon uses SUEWS via `suews-agent` to model urban heat in a synthetic heat-vulnerable city.
- Submissions should produce a heat hazard layer and translate it into a socio-economic heat-risk indicator.
- Public GitHub Pages output is judged for policy relevance and presentation quality.
- Repository contents, SUEWS configuration/results, prompts, and AI transcripts support technical judging.
- Correct SUEWS citation is required.

## SUEWS agent/runtime setup

The `suews-agent` command was not installed on `PATH`, so the upstream plugin repository was cloned for setup instructions:

```powershell
& 'C:\Program Files\Git\cmd\git.exe' clone https://github.com/UMEP-dev/suews-agent.git work\suews-agent
```

The repo explains that the Codex/Claude plugin launches the SUEWS MCP through Python tooling. Because the MCP tool was not exposed directly in this thread and `uv` was not installed, a local Python virtual environment was created under `work/.venv` and the official SUEWS MCP/runtime package was installed from `UMEP-dev/SUEWS`:

```powershell
& 'C:\Users\thaku\.cache\codex-runtimes\codex-primary-runtime\dependencies\python\python.exe' -m venv work\.venv
& 'C:\Users\thaku\.cache\codex-runtimes\codex-primary-runtime\dependencies\python\python.exe' -m pip --python work\.venv install pip
$env:PATH='C:\Program Files\Git\cmd;' + $env:PATH
& 'C:\Users\thaku\Documents\Codex\2026-06-09\you-re-helping-me-get-set-3\work\.venv\Scripts\python.exe' -m pip install "git+https://github.com/UMEP-dev/SUEWS.git#subdirectory=mcp"
```

Verified imports and CLI:

```powershell
& '...\work\.venv\Scripts\python.exe' -c "import supy, suews_mcp; print('supy', getattr(supy, '__version__', 'unknown')); print('suews_mcp ok')"
& '...\work\.venv\Scripts\suews.exe' --help
```

Result:

- `supy 2026.6.5`
- `suews_mcp ok`
- Unified `suews` CLI available with `init`, `validate`, `inspect`, `run`, `diagnose`, and `summarise`.

## SUEWS smoke test

Created a bundled simple urban example:

```powershell
& '...\work\.venv\Scripts\suews.exe' init --template simple-urban --format json analysis\example-run
```

Created files:

- `analysis/example-run/sample_config.yml`
- `analysis/example-run/Kc_2012_data_60.txt`

Validated and inspected:

```powershell
& '...\work\.venv\Scripts\suews.exe' validate --format json analysis\example-run\sample_config.yml
& '...\work\.venv\Scripts\suews.exe' inspect analysis\example-run\sample_config.yml --format json
```

Validation completed with warnings but no errors. The warnings were demo-case/setup warnings, including inactive land-cover parameter blocks and optional wall/roof consistency checks.

Ran the simulation:

```powershell
& '...\work\.venv\Scripts\suews.exe' run analysis\example-run\updated_sample_config.yml
```

Run result:

- Simulation period: `2012-01-01 00:05:00` to `2013-01-01 00:00:00`
- SUEWS version: `2026.6.5`
- Forcing validation passed
- Output written:
  - `analysis/example-run/Output/KCL1_2012_SUEWS_60.txt`
  - `analysis/example-run/Output/KCL_SUEWS_checkpoint.json`
- CLI reported: `SUEWS run successfully done!`

Diagnostics and summary:

```powershell
& '...\work\.venv\Scripts\suews.exe' diagnose analysis\example-run --format json
& '...\work\.venv\Scripts\suews.exe' summarise analysis\example-run
```

Final diagnostic result after adding provenance:

- Passes: 3
- Warnings: 1
- Failures: 0
- Key NaN fractions for `QH`, `QE`, and `QN`: `0.0`
- Remaining warning: mean energy-balance closure residual exceeded the diagnostic threshold in the packaged demo case.

Added:

- `analysis/example-run/provenance.json`
- `analysis/example-run/SMOKE_TEST.md`

## GitHub Pages

Checked Pages configuration:

```powershell
& 'C:\Program Files\GitHub CLI\gh.exe' api repos/divya42581/suews-hackathon-practice/pages
```

Result:

- Pages status: `built`
- Source: `main`, `/docs`
- Public: `true`
- Pages URL: <https://divya42581.github.io/suews-hackathon-practice/>

Verified the Pages URL with a direct request:

```powershell
Invoke-WebRequest -Uri 'https://divya42581.github.io/suews-hackathon-practice/' -UseBasicParsing
```

Result:

- HTTP status: `200`

## Files changed in this setup

- `analysis/example-run/Kc_2012_data_60.txt`
- `analysis/example-run/sample_config.yml`
- `analysis/example-run/updated_sample_config.yml`
- `analysis/example-run/report_sample_config.txt`
- `analysis/example-run/report_sample_config.json`
- `analysis/example-run/Output/KCL1_2012_SUEWS_60.txt`
- `analysis/example-run/Output/KCL_SUEWS_checkpoint.json`
- `analysis/example-run/provenance.json`
- `analysis/example-run/SMOKE_TEST.md`
- `transcripts/session-setup-2026-06-09.md`

## Status

- Repository: public repo exists and is cloned locally.
- Task brief: read.
- SUEWS smoke test: completed end to end with output files and diagnostics.
- GitHub Pages: public, built from `main` and `/docs`, verified HTTP 200.
- Transcript: saved in `transcripts/`.
