# Heat Hazard and Urban Flux Partitioning: SUEWS Practice Analysis

## Main Message: Heat Hazard Was Driven by Stored Urban Heat

This practice analysis used the bundled KCL/London SUEWS sample case to test the
hackathon workflow. The main finding is that the selected heat-hazard period was
not only warmer in the air; the urban surface stored much more heat, which can
help sustain heat after the daytime peak.

This is a practice run, not the final hackathon-city analysis. It shows how a
SUEWS result can be turned into a public story with transparent caveats.

## Aim: Turn a Model Run into a Heat-Hazard Story

### What we wanted to check

- Can the SUEWS toolchain run end to end?
- Can hourly SUEWS output be used to define a simple heat hazard?
- During the hazard period, where did the available energy go?
- What extra information is needed to move from heat hazard to heat risk?

## Method: Use a Simple Threshold and Compare Before, During, and After

### Simulation setup

The run used SUEWS/SuPy version `2026.6.5` with the packaged KCL/London
`simple-urban` sample case and 2012 hourly forcing. The analysed output was:

```text
analysis/demo-simple-urban/Output/KCL1_2012_SUEWS_60.txt
```

### Hazard definition

Heat hazard was calculated as hourly exceedance above `28 C`:

```text
degree-hours = sum(max(T2 - 28 C, 0))
```

Apparent temperature was also calculated to include humidity and wind:

```text
apparent_temperature = T2 + 0.33e - 0.70U10 - 4
```

### Comparison windows

The heat-hazard period was the longest run of days with daily maximum
`T2 >= 28 C`.

- Pre-hazard: `2012-08-14` to `2012-08-16`
- Hazard: `2012-08-17` to `2012-08-19`
- Post-hazard: `2012-08-20` to `2012-08-22`

Daytime fluxes were averaged when `Kdown > 20 W m-2`.

## Result 1: The Workflow Ran, but This Is Still a Practice Case

### Toolchain check

The SUEWS run produced hourly output with `0%` missing values in `QN`, `QH`, and
`QE`. Diagnostics reported `3` passes, `1` warning, and `0` failures.

### Interpretation boundary

The remaining warning was an energy-balance closure warning. That means the run
is useful for learning the workflow, but it should not be treated as a
publication-ready climate result.

## Result 2: Humidity Made the Heat Hazard Stronger

### Air temperature alone understated the event

During the selected heat-hazard period:

- Maximum `T2`: `30.4 C`
- Hours with `T2 >= 28 C`: `14`
- `T2` degree-hours above `28 C`: `18.5 C-hours`

### Apparent temperature showed higher heat stress

When humidity and wind were included:

- Maximum apparent temperature: `32.4 C`
- Hours with apparent temperature `>= 28 C`: `25`
- Apparent-temperature degree-hours above `28 C`: `53.1 C-hours`

<p align="center">
  <img src="assets/heat_hazard_timeseries.png" alt="Heat hazard threshold and degree-hours" width="82%">
  <br>
  <em>Figure 1. Heat-hazard identification using daily maximum T2, apparent temperature, and degree-hours above 28 C.</em>
</p>

## Result 3: The Hazard Period Had Much More Available Energy

### Daytime energy input increased sharply

Daytime `QN + QF` increased from `248.5 W m-2` before the hazard period to
`329.3 W m-2` during the hazard period. After the event it returned to
`251.3 W m-2`.

### The comparison was balanced

Each period contains `72` hourly records and about `41-42` daytime hours, so the
before/during/after comparison is like-for-like.

<p align="center">
  <img src="assets/hazard_period_flux_timeseries.png" alt="Heat hazard period with energy and water fluxes" width="82%">
  <br>
  <em>Figure 2. Hourly temperature, energy fluxes, and water fluxes for the pre-hazard, hazard, and post-hazard windows.</em>
</p>

## Result 4: Extra Energy Went Mainly into Storage and Sensible Heat

### Storage heat was the biggest change

Daytime storage heat `QS` nearly doubled, from `59.2 W m-2` before the event to
`110.6 W m-2` during it. Sensible heat `QH` also rose, while latent heat `QE`
rose only slightly.

| Period | Daytime QH | Daytime QE | Daytime QS | Daytime QN + QF |
| --- | ---: | ---: | ---: | ---: |
| Pre-hazard | 144.1 | 45.2 | 59.2 | 248.5 |
| Hazard | 164.5 | 54.1 | 110.6 | 329.3 |
| Post-hazard | 154.7 | 36.6 | 59.9 | 251.3 |

### The hazard-period partition was less evaporative

During the hazard period, daytime `QH + QE + QS` was partitioned as:

- `50.0%` sensible heat
- `16.4%` latent heat
- `33.6%` storage heat

The storage fraction was higher than both the pre-hazard and post-hazard
periods, which were each about `23.8%`.

<p align="center">
  <img src="assets/pre_during_post_flux_partition.png" alt="Daytime flux partitioning before, during, and after the hazard period" width="62%">
  <br>
  <em>Figure 3. Daytime mean flux partitioning, showing the rise in storage heat during the hazard period.</em>
</p>

## Discussion: Hazard Is Not the Same as Risk

### What SUEWS gives us

SUEWS provides the physical hazard layer: air temperature, apparent heat,
radiation, sensible heat, latent heat, storage heat, rainfall, evaporation, and
runoff.

### What risk still needs

A complete heat-risk indicator also needs exposure, vulnerability, and adaptive
capacity. A simple structure for later processing is:

```text
risk_index = hazard_index * exposure_index * vulnerability_index *
             (1 - adaptive_capacity_index)
```

Useful non-SUEWS indicators include population exposed, age structure,
deprivation or low-income share, outdoor-worker share, housing quality, baseline
health vulnerability, access to cooling, and green/blue-space access.

## Conclusion: The Workflow Is Ready for the Real Hackathon Dataset

This practice run shows how to move from SUEWS output to a readable heat-hazard
story. In this sample case, the heat-hazard period was marked by higher air
temperature, stronger apparent heat, and a clear increase in storage heat.

For the final hackathon submission, the same workflow should be repeated with
the focus-city dataset. The hazard result should then be combined with
socio-economic indicators to estimate heat risk, with assumptions stated
clearly.

## Citation

This practice run used SUEWS/SuPy version `2026.6.5`. The final hackathon
submission should cite the exact software version used, following the current
[SUEWS citation guidance](https://docs.suews.io/stable/#how-to-cite-suews).

Core model references:

- Jarvi, L., Grimmond, C.S.B. and Christen, A. (2011). The Surface Urban Energy
  and Water Balance Scheme (SUEWS): Evaluation in Los Angeles and Vancouver.
  *Journal of Hydrology*, 411(3-4), 219-237.
  [https://doi.org/10.1016/j.jhydrol.2011.10.001](https://doi.org/10.1016/j.jhydrol.2011.10.001)
- Ward, H.C., Kotthaus, S., Jarvi, L. and Grimmond, C.S.B. (2016). Surface
  Urban Energy and Water Balance Scheme (SUEWS): Development and evaluation at
  two UK sites. *Urban Climate*, 18, 1-32.
  [https://doi.org/10.1016/j.uclim.2016.05.001](https://doi.org/10.1016/j.uclim.2016.05.001)
