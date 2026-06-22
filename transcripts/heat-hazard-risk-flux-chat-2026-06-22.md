# Heat hazard, risk, and flux-partitioning discussion transcript

Date: 2026-06-22

This transcript records the follow-up analysis after the initial SUEWS smoke
test.

## Questions asked

1. How should heat, hazard, and risk be understood from the simulation?
2. If simulation alone is not sufficient for risk, what indicators or formulas
   should be used?
3. How is hazard calculated in a time series?
4. During the heat-hazard period, how was energy partitioned between sensible
   heat, latent heat, and storage heat?
5. How did that compare with pre-heat-hazard and post-heat-hazard days?

## Indicators chosen

The simulation output was treated as the heat-hazard layer, not the full risk
layer. Risk needs additional exposure, vulnerability, and adaptive-capacity
data.

Hazard metric:

```text
degree-hours = sum(max(T2 - 28 C, 0))
```

Humid-heat metric:

```text
apparent_temperature = T2 + 0.33e - 0.70U10 - 4
```

Risk bridge for later processing:

```text
risk_index = hazard_index * exposure_index * vulnerability_index *
             (1 - adaptive_capacity_index)
```

Recommended non-SUEWS indicators: population exposed, age structure,
deprivation or low-income share, outdoor-worker share, housing quality,
pre-existing health vulnerability, access to cooling, and green/blue-space
access.

## Heat-hazard period selected

The hazard period was selected as the longest run of days where daily maximum
`T2 >= 28 C`:

- Pre-hazard: 2012-08-14 to 2012-08-16
- Hazard: 2012-08-17 to 2012-08-19
- Post-hazard: 2012-08-20 to 2012-08-22

During the hazard period:

- Mean `T2`: `25.0 C`
- Max `T2`: `30.4 C`
- Hours with `T2 >= 28 C`: `14`
- `T2` degree-hours above `28 C`: `18.5 C-hours`
- Hours with apparent temperature `>= 28 C`: `25`
- Apparent-temperature degree-hours above `28 C`: `53.1 C-hours`

## Flux-partitioning result

Daytime means, using `Kdown > 20 W m-2`:

| Period | QH sensible | QE latent | QS storage | QN + QF |
| --- | ---: | ---: | ---: | ---: |
| Pre-hazard | 144.1 | 45.2 | 59.2 | 248.5 |
| Hazard | 164.5 | 54.1 | 110.6 | 329.3 |
| Post-hazard | 154.7 | 36.6 | 59.9 | 251.3 |

Interpretation: during the heat-hazard window, the largest change was storage
heat. `QS` nearly doubled compared with the pre- and post-periods. `QH` also
increased, while `QE` increased only modestly. In plain terms, the extra
available energy mostly heated the urban fabric and the air rather than being
used for evaporation.

## Files created

- `docs/assets/heat_hazard_timeseries.png`
- `docs/assets/hazard_period_flux_timeseries.png`
- `docs/assets/pre_during_post_flux_partition.png`
- `analysis/demo-simple-urban/heat_hazard_metrics.csv`
- `analysis/demo-simple-urban/heat_hazard_processing_summary.json`
- `analysis/demo-simple-urban/heat_hazard_flux_partition_summary.csv`

## Caveat

This remains the packaged KCL/London practice run. It is useful for learning the
workflow and interpretation, but it is not the final hackathon-city analysis.
