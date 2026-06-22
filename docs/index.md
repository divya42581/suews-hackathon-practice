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

## Heat hazard and flux partitioning

For this practice analysis, heat hazard was calculated from hourly model output
using a transparent temperature threshold:

```text
degree-hours = sum(max(T2 - 28 C, 0))
```

The selected heat-hazard period is 2012-08-17 to 2012-08-19, the longest run of
days in the sample with daily maximum `T2 >= 28 C`. It is compared with equal
three-day windows before and after:

- Pre-hazard: 2012-08-14 to 2012-08-16
- Hazard: 2012-08-17 to 2012-08-19
- Post-hazard: 2012-08-20 to 2012-08-22

During the hazard period, the modelled air temperature reached `30.4 C`, with
`14` hours at `T2 >= 28 C`. Apparent temperature exceeded `28 C` for `25` hours,
showing that humidity made the heat stress feel stronger than air temperature
alone.

![Heat hazard threshold and degree-hours](assets/heat_hazard_timeseries.png)

The flux comparison shows that the heat-hazard period had much higher daytime
available energy. Daytime `QN + QF` increased from about `249 W m-2` before the
event to `329 W m-2` during it. The largest partitioning change was storage
heat: daytime `QS` rose from about `59 W m-2` before the event to `111 W m-2`
during it. Sensible heat `QH` also increased, while latent heat `QE` increased
only modestly.

| Period | Daytime QH | Daytime QE | Daytime QS | Daytime QN + QF |
| --- | ---: | ---: | ---: | ---: |
| Pre-hazard | 144.1 | 45.2 | 59.2 | 248.5 |
| Hazard | 164.5 | 54.1 | 110.6 | 329.3 |
| Post-hazard | 154.7 | 36.6 | 59.9 | 251.3 |

![Heat hazard period with energy and water fluxes](assets/hazard_period_flux_timeseries.png)

![Daytime flux partitioning before, during, and after the hazard period](assets/pre_during_post_flux_partition.png)

The interpretation is that the hazard period was not just hotter; the urban
surface stored substantially more heat. This is the mechanism that matters for
heat persistence and warmer nights in dense urban settings.

## Risk indicator boundary

The SUEWS run provides the heat-hazard side of risk. It does not by itself
provide exposure, vulnerability, or adaptive capacity. A simple bridge for later
hackathon processing is:

```text
risk_index = hazard_index * exposure_index * vulnerability_index *
             (1 - adaptive_capacity_index)
```

Useful non-SUEWS indicators to add are population exposed, age structure,
income or deprivation, outdoor-worker share, housing quality, baseline health
vulnerability, access to cooling, and green/blue-space access.

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
